# ライフサイクルとブートストラップのパターン

## 問題

エージェントランタイムには、安全性を損なわずに拡張できる余地が必要です。

- **Hooks** — ライフサイクル上の節目で動作を拡張する（ツール実行の前後、セッション開始/終了）
- **Background tasks** — メインのエージェントを止めずに、長時間かかる作業を追跡する
- **Bootstrap** — 複数の起動モード（CLI、server、SDK）にまたがって初期化を構造化する

しかし、制御されていない拡張性は次の問題を生みます。
- 信頼できない hooks によるセキュリティホール
- 完了しない task によるリソースリーク
- 初期化時の race condition

## 基本ルール

### Hook の信頼は全体か無か

workspace が信頼されていない場合は、**すべての hooks をスキップ**します。疑わしいものだけを除外するわけではありません。session スコープの hooks は一時的なもので、session 終了時にクリーンアップされます。

```typescript
// 例: trust gate を持つ hook ディスパッチ
async function dispatchHook(
  hookType: HookType,
  context: HookContext
): Promise<HookResult[]> {
  
  // trust gate: workspace が信頼されていなければ、すべての hooks をスキップする
  if (!context.trustBoundary.crossed) {
    logger.warn('信頼されていない workspace のため、hooks をスキップします');
    return [];
  }
  
  // session スコープの hooks は一時的なもの。session 終了時にクリーンアップする
  const sessionHooks = context.hooks.getByScope('session');
  const projectHooks = context.hooks.getByScope('project');
  
  return await Promise.all([
    ...sessionHooks.map(h => h.execute(context)),
    ...projectHooks.map(h => h.execute(context)),
  ]);
}
```

### 長時間作業: 2 段階 eviction を伴う型付き state machine

各 work unit には次が与えられます。
1. **型付きで prefix 付きの ID**（例: `extractor-001`, `benchmark-002`）
2. **厳密なライフサイクル**（running → completed | failed | killed）
3. **disk-backed な出力**（単なる in-memory ではない）

eviction は 2 段階です。
1. **disk 上の出力** は terminal state で即座にクリーンアップする
2. **in-memory の記録** は parent への通知後に遅延クリーンアップする

### Bootstrap: 依存順で並ぶ、メモ化された stage

複数の entry mode（CLI、server、SDK）は同じ bootstrap path を共有します。

```
Stage 1: 最小限のコンテキストを作成する（trust は不要）
  ↓
Stage 2: tools を読み込む（read-only で安全）
  ↓
Stage 3: trust boundary を越える（user が consent を与える）
  ↓
Stage 4: セキュリティ上センシティブな subsystem を読み込む（telemetry、secret env vars）
```

**重要な分岐点**: セキュリティ上センシティブな subsystem は、trust が確立する前に有効化してはなりません。

## 使う場面

- core code を変更せずに agent の挙動を拡張したい
- 長時間動く background work を追跡したい
- 複数の entry mode にまたがる構造化された初期化が必要
- ライフサイクル上の節目（pre/post tool、session start/end）で hooks が必要

## トレードオフ

| 決定 | 利点 | コスト |
|---|---|---|
| Hook の信頼を全体か無かにする | セキュリティ境界が単純 | 信頼できない hook が 1 つあるだけで拡張システム全体が無効になる |
| disk-backed な task 出力 | concurrent work に関係なく memory 使用量が一定 | I/O latency が work unit 数に比例する |
| 依存順の bootstrap | 複数の entry mode で path を共有できる | 初期 startup が直列になる（stage を並列化できない） |
| メモ化された stage | 再初期化が高速 | config 変更時に memoization を慎重に無効化する必要がある |

## 実装パターン

### Hook のライフサイクル

定義されたタイミングで 6 種類の hook がディスパッチされます。

```typescript
interface HookRegistry {
  // session のライフサイクル
  onSessionStart: (context: SessionContext) => Promise<void>;
  onSessionEnd: (context: SessionContext) => Promise<void>;
  
  // tool 実行
  preToolExecute: (context: ToolContext) => Promise<ToolContext>;
  postToolExecute: (context: ToolResult) => Promise<ToolResult>;
  
  // prompt 送信
  prePromptSubmit: (context: PromptContext) => Promise<PromptContext>;
  postPromptSubmit: (context: ResponseContext) => Promise<ResponseContext>;
}

// 使い方: config 経由で hooks を登録する
// /update-config hooks.preToolExecute = "scripts/audit-tool-call.js"
```

### 長時間 task の追跡

```typescript
interface TaskRegistry {
  // 型付き prefix ID
  registerWork(
    type: 'extraction' | 'benchmark' | 'indexing',
    outputType: 'json' | 'text' | 'file'
  ): string; // 型付き ID を返す: `extraction-001`
  
  // 厳密な state machine
  updateState(
    taskId: string,
    state: 'running' | 'completed' | 'failed' | 'killed',
    output?: any
  ): void;
  
  // 2 段階 eviction
  evictTask(taskId: string): void;
  // 1. disk 出力をクリーンアップする（terminal state で即時）
  // 2. in-memory の記録をクリーンアップする（parent 通知後に遅延）
}
```

### Bootstrap の流れ

```typescript
// 例: 依存順の初期化
class AgentBootstrap {
  private stages = new Map<string, Stage>();
  private memoizedCallers = new Map<string, any>();
  
  async bootstrap(entryMode: 'cli' | 'server' | 'sdk'): Promise<AgentContext> {
    
    // Stage 1: 最小限のコンテキスト（trust は不要）
    await this.runStage('minimal-context', async () => {
      return {
        cwd: process.cwd(),
        entryMode,
        trustBoundary: { crossed: false },
      };
    });
    
    // Stage 2: tools を読み込む（read-only なら安全）
    await this.runStage('load-tools', async (context) => {
      context.tools = await this.loadSafeTools();
      return context;
    });
    
    // Stage 3: trust boundary（user が consent を与える）
    await this.runStage('trust-boundary', async (context) => {
      const consent = await this.requestConsent();
      context.trustBoundary = { crossed: consent };
      return context;
    });
    
    // Stage 4: セキュリティ上センシティブな subsystem（trust が必要）
    if (context.trustBoundary.crossed) {
      await this.runStage('load-sensitive', async (context) => {
        context.telemetry = await this.loadTelemetry();
        context.secretEnvVars = await this.loadSecrets();
        return context;
      });
    }
    
    return context;
  }
  
  private async runStage(
    name: string,
    fn: (context: AgentContext) => Promise<AgentContext>
  ): Promise<void> {
    // メモ化済み: すでに実行済みならスキップ
    if (this.stages.has(name) && this.stages.get(name).complete) {
      return;
    }
    
    // stage を実行する
    const stage = { name, complete: false, running: true };
    this.stages.set(name, stage);
    
    try {
      await fn(this.context);
      stage.complete = true;
    } finally {
      stage.running = false;
    }
  }
}
```

## 落とし穴

1. **Hook の信頼は全体か無か** — 信頼できない hook が 1 つあるだけで拡張システム全体が無効になる
2. **ほとんどの async work は "pending" state を飛ばす** — work unit は直接 "running" として登録される
3. **eviction には通知が必要** — terminal な work unit は parent に通知されて初めて GC 対象になる
4. **fast-path dispatch** — メモ化された caller は stage を再実行せずに concurrent call を処理しなければならない
5. **hook type は互いに重なってはいけない** — 重複する hook scope を作らない

## 関連パターン

- [Tool Registry](tool-registry-pattern.md) — bootstrap 時に tools をどう登録するか
- [Memory Persistence](memory-persistence-pattern.md) — init 時に memory をどう読み込むか

## テンプレート: Bootstrap チェックリスト

bootstrap 完了を宣言する前に確認します。

```markdown
## Bootstrap の確認

### Stage 1: 最小限のコンテキスト
- [ ] 作業ディレクトリを確認済み
- [ ] 起動モードを判定済み（cli / server / sdk）
- [ ] 信頼境界はまだ越えていない（secret は読み込まない）

### Stage 2: Tools を読み込み済み
- [ ] read-only の tools を登録済み（read, search, glob）
- [ ] write tools はまだ登録していない（edit, shell）
- [ ] tool permission は default に設定済み（ask / deny）

### Stage 3: 信頼境界
- [ ] user consent を要求済み（interactive または config flag）
- [ ] consent を session state に記録済み
- [ ] security audit を記録済み

### Stage 4: センシティブなサブシステム
- [ ] telemetry を初期化済み（consent がある場合）
- [ ] secret env vars を読み込み済み（consent がある場合）
- [ ] write tools を登録済み（edit, shell, exec）
- [ ] hook system を有効化済み（workspace が信頼されている場合）

### Stage 5: バックグラウンドタスク
- [ ] task registry を初期化済み
- [ ] cleanup handler を登録済み
- [ ] shutdown 時に drain する設定を済ませた

## いずれかの Stage が失敗した場合

- Bootstrap はただちに停止する
- session は safe mode のまま維持する（read-only）
- error を stage 名と失敗理由付きで記録する
```

## 根拠

ライフサイクルと bootstrap のパターンは、次のような本番 runtime で確認されています。
- workspace の信頼に基づき、hook dispatch は全件有効か全件無効かのどちらかになる
- 長時間タスクは、型付きの prefix 付き ID と disk-backed な出力を使う
- Bootstrap は依存順に並び、stage はメモ化される
- trust boundary は、セキュリティ上センシティブな subsystem に対する明確な転換点になる
