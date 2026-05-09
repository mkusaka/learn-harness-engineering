# マルチエージェント協調パターン

## 問題

単一エージェントには限界があります:
- **コンテキストの制約** — 1 回のセッションで、調査と実装の全体を保持できない
- **専門化** — 調査担当、実装担当、レビュー担当を分ける必要がある
- **並列性** — 複数のアプローチを同時に検討したい

しかし、マルチエージェントシステムは混乱も生みます:
- 作業者同士が同じ調査を重複して行う
- コーディネーターが理解を統合せず、そのまま委任してしまう
- コンテキスト継承が指数関数的に膨らむ

## 基本原則

### コーディネーターは理解を委任せず、統合する

**アンチパターン:**
> "Based on your findings, fix the authentication system."

**パターン:**
> "Research identified 3 auth flows: login, logout, token refresh. Implement ONLY the token refresh handler using the JWT strategy documented in [research output]. Return: implementation diff + test results."

コーディネーター（オーケストレーター）の価値は、作業者の結果を要約して正確な仕様に落とし込み、それから実装を割り振ることにあります。

### 3 つの委任パターン

| パターン | コンテキスト共有 | 適している用途 | 制約 |
|---------|----------------|----------|-------------|
| **Coordinator** | なし — 作業者は新規状態から開始 | 複数段階の複雑なタスク（調査 → 統合 → 実装 → 検証） | 最も遅いが最も安全 |
| **Fork** | 完全 — 子は親の履歴を継承する | 読み込んだコンテキストを共有した、手早い並列分割 | **1 階層のみ** — 再帰的な fork はコンテキストコストを増大させる |
| **Swarm** | 共有タスクリストを介したピアツーピア | 長時間にわたる独立した作業ストリーム | **フラットな名簿** — メンバーは他のメンバーを spawn できない |

### 結果は非同期で届く; fire-and-forget の登録は ID を即座に返す

```typescript
// 例: 作業者を spawn すると、ID がすぐに返る
const taskId = await coordinator.spawn({
  type: 'research',
  prompt: 'Analyze auth flows...',
  toolFilter: ['read', 'search'], // ツールを制限する
});

// 作業者が動いている間も、親は作業を続けられる
// 結果は callback か polling で届く
```

## 使いどころ

- タスクが単一エージェントのセッションには大きすぎる
- 並列探索が必要なとき（例: 複数のアプローチを試作する）
- 専門化されたチームメンバーを持続的に使いたいとき（researcher、implementer、reviewer）
- 複雑な多段階ワークフロー

## トレードオフ

| パターン | 速度 | 安全性 | コンテキストコスト |
|---------|-------|--------|--------------|
| **Coordinator** | 最も遅い | 最も安全 | 最小（継承なし） |
| **Fork** | 最速 | 中 | 最大（完全継承） |
| **Swarm** | 中 | 中 | 中（共有状態のみ） |

## 実装パターン

### Coordinator パターン（複雑なタスクに推奨）

段階的なワークフロー:

```
フェーズ 1: 調査
  ↓ (発見を統合)
フェーズ 2: 計画
  ↓ (正確な仕様)
フェーズ 3: 実装
  ↓ (検証)
フェーズ 4: レビュー
```

```typescript
// 例: Coordinator ワークフロー
const research = await coordinator.spawn({
  role: 'researcher',
  prompt: `Analyze existing authentication in ${authDir}.
  Find: login flow, logout flow, token handling.
  Return: structured findings only. NO implementation suggestions.`,
  toolFilter: ['read', 'search', 'glob'], // 書き込み不可
});

await coordinator.synthesize(research.results);

const implement = await coordinator.spawn({
  role: 'implementer',
  prompt: `Implement token refresh handler using the JWT strategy
  from [Phase 2 findings]. 
  Constraints: Use existing AuthService patterns, add tests.`,
  toolFilter: ['read', 'search', 'edit', 'test'], // 書き込み可
});
```

### Fork パターン（1 階層のみ）

```typescript
// 親が子を spawn して並列作業を行う
const forks = await Promise.all([
  coordinator.fork({
    prompt: 'Implement login handler',
    inheritContext: true, // 親の履歴をすべて継承
  }),
  coordinator.fork({
    prompt: 'Implement logout handler',
    inheritContext: true,
  }),
]);

// 重要: 子は再帰的に fork してはいけない
// 許すとコンテキストコストが親 + child1 + child2 + ... のように増える
```

### Swarm パターン（フラットな名簿）

```typescript
// Swarm: 共有タスクリストを持つ永続的なチーム
const swarm = new Swarm([
  { id: 'researcher', specialty: 'research' },
  { id: 'implementer', specialty: 'implementation' },
  { id: 'reviewer', specialty: 'verification' },
]);

// エージェントは共有キューからタスクを拾う
// 結果は共有状態に書き戻される
await swarm.dispatch({
  taskId: 'feat-001',
  pickedBy: 'implementer',
});
```

## 注意点

1. **Fork の子は fork してはいけない** — 再帰ガードにより 1 階層の不変条件を保つ。fork ツールは子のプールに残しておく（プロンプトキャッシュ共有のため）が、呼び出し時にブロックする。
2. **Coordinator の作業者はコンテキスト 0 から始まる** — 明示的に渡したプロンプトだけが使われる。子が親の蓄積した調査結果を見ていると仮定しないこと。
3. **Swarm のメンバーは他のメンバーを spawn できない** — 制御不能な増殖を防ぐため、名簿はフラットにする。
4. **自己完結したプロンプトを書く** — "Based on your findings" はアンチパターン。コーディネーターが先に要約しなければならない。
5. **各作業者のツールセットを絞る** — researcher に write は不要、implementer に広範な search は不要。

## 関連パターン

- [Context Engineering](context-engineering-pattern.md) — 委任のための分離パターン
- [Lifecycle & Bootstrap](lifecycle-bootstrap-pattern.md) — エージェントが init 時にどう spawn されるか

## テンプレート: Worker Prompt Structure

```markdown
# 自己完結した Worker Prompt

## Context (Coordinator の統合結果をコピー)

**Task**: トークン更新ハンドラを実装する
**Background**: 調査の結果、JWT ベースの認証で 24h の access token を使っていることが分かった。
**Decision**: refresh token rotation を使う（refresh のたびに新しい refresh token を発行する）。

## Your Role

あなたは **implementer** です。上記の仕様に従って本番コードを書くのがあなたの役割です。

## Constraints

- `${authServicePath}` の既存パターンを使う
- 成功ケースと失敗ケースのテストを追加する
- login/logout ハンドラは変更しない（別タスク）

## Your Tools

- read, search, edit, test
- Shell: npm test, npm run check のみ

## Deliverable

Return:
1. 実装 diff（変更したファイル）
2. テスト結果（pass/fail）
3. 必要なブロッカーまたは確認事項

**Do NOT return**: 調査結果、アーキテクチャ上の議論、代替設計。
```

## 根拠

マルチエージェントの協調パターンは、次のような本番システムで確認されています:
- Coordinator の作業者はコンテキスト継承が 0 の状態から始まる
- コンテキスト爆発を抑えるため、fork は 1 階層に制限される
- Swarm のエージェントは、直接プロンプトではなく共有タスクリストを通じて通信する
- 結果は fire-and-forget の登録とともに非同期で届く
