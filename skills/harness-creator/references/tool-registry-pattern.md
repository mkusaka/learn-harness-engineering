# ツールレジストリと安全性のパターン

## 問題

エージェントが生産的に動くには、`shell`、ファイル編集、検索などのツールが必要です。ただし、制限のないツールアクセスには次のようなリスクがあります。

- 破壊的な操作（`rm -rf`、`DROP TABLE` など）
- ツール呼び出しの並行実行による競合状態
- 権限設定の誤りによる、気づきにくいポリシー違反

解決策は、明示的な並行実行分類と複数ソースの権限パイプラインを備えた、**fail-closed なレジストリ**です。

## 基本ルール

### デフォルトは fail-closed にする

ツールは、安全であると明示的に指定されない限り、**非並行**かつ**非読み取り専用**として扱います。これにより、次を防げます。
- 状態を変更する操作の意図しない並列実行
- 並行書き込みによる、気づきにくいデータ破損

### 並行性はツール単位ではなく呼び出し単位で判定する

同じツールでも、入力によって安全な場合と危険な場合があります。

```
✓ 安全（並列実行可）:
  - cat file1.txt
  - grep "pattern" src/
  - ls -la

✗ 危険（直列実行が必要）:
  - rm -rf build/
  - npm install (network, filesystem mutation)
  - sed -i 's/old/new/g' *.ts
```

ランタイムは、ツール呼び出しのバッチを連続するグループに分割します。安全な呼び出しは並列実行され、危険な呼び出しがあるとそこで直列セグメントが始まります。

### 権限パイプラインには副作用がある

権限評価器は**状態を持つ**もので、次の処理を行います。
- 拒否を記録する（監査とレート制限のため）
- モードを変換する（例: `auto` → 拒否後に `ask`）
- 副作用としてセッション状態を更新する

**厳密な優先順位は次のとおりです。**
```
ポリシー（組織全体） → ユーザー設定 → プロジェクトルール → ローカルの上書き設定 → セッショングラント
```

## 使う場面

- エージェントランタイムにツール登録が必要なとき
- 並列ツール呼び出しの並行制御が必要なとき
- 権限ゲート（自動承認、事前確認、拒否）が必要なとき
- 監査のためにツール使用状況を追跡したいとき

## トレードオフ

| 判断 | 利点 | コスト |
|---|---|---|
| fail-closed のデフォルト | 新しいツールを最初から安全に扱える | 開発者が並行実行を明示的に有効化する必要がある |
| 呼び出し単位の分類 | 並行実行を細かく制御できる | ツール登録だけでなく、各呼び出しの解析が必要になる |
| 複数ソースの権限レイヤリング | 柔軟にポリシーを組み合わせられる | ルールが衝突したときに原因を追いにくい |
| 状態を持つ評価器 | 履歴に応じて挙動を変えられる | 純粋関数ではないため、テストしづらい |

## 実装パターン

### ツール登録

```typescript
// 例: ツールレジストリエントリ
interface ToolDefinition {
  name: string;
  description: string;
  handler: (args: any) => Promise<any>;
  
  // 安全性の分類
  isReadOnly: boolean;       // デフォルト: false
  isConcurrentSafe: boolean; // デフォルト: false
  
  // 任意のカスタム権限ロジック
  permissionCheck?: (args: any, context: ToolContext) => PermissionResult;
}

// ツールを登録する
registry.register('read_file', {
  name: 'read_file',
  description: 'ファイルの内容を読む',
  handler: readFile,
  isReadOnly: true,
  isConcurrentSafe: true,  // 複数ファイルを並列で読むのは安全
});

registry.register('write_file', {
  name: 'write_file',
  description: 'ファイルを書き込む、または上書きする',
  handler: writeFile,
  isReadOnly: false,
  isConcurrentSafe: false, // 競合状態を防ぐため、直列で実行する必要がある
});
```

### 権限パイプライン

```typescript
// 権限評価の順序
async function evaluatePermission(
  toolCall: ToolCall,
  context: PermissionContext
): Promise<PermissionResult> {
  
  // 1. ポリシールール（最優先、組織全体）
  const policyResult = await policyEngine.check(toolCall, context);
  if (policyResult !== 'defer') return policyResult;
  
  // 2. ユーザー設定
  const userResult = await userSettings.check(toolCall, context);
  if (userResult !== 'defer') return userResult;
  
  // 3. プロジェクトルール
  const projectResult = await projectRules.check(toolCall, context);
  if (projectResult !== 'defer') return projectResult;
  
  // 4. ローカルの上書き設定
  const localResult = await localOverrides.check(toolCall, context);
  if (localResult !== 'defer') return localResult;
  
  // 5. セッショングラント（最下位の優先度）
  return sessionGrants.check(toolCall, context);
}
```

### バイパス不能ルール

特定のパスや操作は、決して自動承認してはいけません。

```yaml
# 保護されたパス（自動承認しない）
protected_paths:
  - /etc/**
  - /usr/**
  - node_modules/**
  - .git/**

# 保護されたコマンド（常に確認する）
protected_commands:
  - "rm -rf*"
  - "DROP TABLE*"
  - "DELETE FROM*"
  - "mkfs*"
```

## 注意点

1. **ほとんどの非同期ワークは "pending" 状態を飛ばす** — ワークユニットは直接 "running" として登録される
2. **権限評価には副作用がある** — 呼び出しをまたいで結果をキャッシュしないこと
3. **並行性の分類には入力の解析が必要** — ツール名だけでは不十分
4. **ツールのデフォルト権限は "allow"** — カスタムロジックのないツールはルールベースのシステムに委譲される
5. **追い出しには通知が必要** — 終端のワークユニットは、親に通知されてから GC 対象になる

## 関連パターン

- [Lifecycle & Bootstrap](lifecycle-bootstrap-pattern.md) — 初期化時にツールがどう登録されるか
- [Hook Lifecycle](hook-lifecycle-pattern.md) — ツール実行前後のフック

## テンプレート: ツール安全性チェックリスト

新しいツールを有効化する前に確認します。

```markdown
## ツール安全性レビュー

**ツール名**: [例: execute_shell]

### 分類
- [ ] 読み取り専用かどうかを判断した（true / false / args に依存）
- [ ] 並行実行可能かどうかを判断した（true / false / args に依存）
- [ ] 安全でない入力パターンを文書化した

### 権限要件
- [ ] デフォルトモードを "ask" または "deny" に設定した
- [ ] バイパス不能なパス/コマンドを定義した
- [ ] 必要に応じてカスタム権限ロジックを実装した
- [ ] 監査ログを有効にした

### テスト
- [ ] 安全な入力でテストした（自動承認されるはず）
- [ ] 危険な入力でテストした（確認または拒否されるはず）
- [ ] 並行実行をテストした（危険なら直列化されるはず）
- [ ] エラーハンドリングをテストした（失敗が記録され、状態が整合すること）
```

## 根拠

ツールレジストリと安全性のパターンは、次のような本番のエージェントランタイムで確認されています。
- 明示的な並行実行フラグを持つ Claude Code のツールレジストリ
- 複数ソースの権限評価（設定 → プロジェクト → セッション）
- 自動承認モードを回避する保護パス/コマンド一覧
- ツールバッチを分割する、呼び出し単位の並行性分類
