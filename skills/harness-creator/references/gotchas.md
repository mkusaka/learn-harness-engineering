# 落とし穴 — Harness Engineering の失敗パターン

見落としやすいが、破るとバグにつながる原則。

---

## 1. Memory Index の上限は静かに発動する

**症状**: 直近のメモリがエラーなしで「消える」。

**原因**: インデックスにはハードな上限があり（例: 200 行 / 25KB）、読み込み時に強制される。長いエントリ（複数文の要約）は、行数の上限内でもバイト上限に達する。

**対策**: インデックスのエントリは 1 行のフックにとどめる。詳細は topic ファイルに置く。

```markdown
✓ Good: "Use bun, not npm - user preference 2024-01-15"
✗ Bad: "The user prefers bun over npm because it's faster. This was discussed on 2024-01-15 when the user said 'use bun not npm' and I updated the package.json accordingly..."
```

---

## 2. 優先順位の順序は直感に反する

**症状**: グローバルなルールがローカルファイルに静かに上書きされる。

**原因**: ローカルの上書きは project ルールより強く、project ルールは user ルールより強く、user ルールは org ルールより強い。user レベルに入れれば最優先されると思っていても、project ルートのローカル上書きファイルが勝つ。

**対策**: instruction ファイルのスタック全体がある状態でテストする。

```bash
# 優先順位の順序をテストする
cat ~/.claude/CLAUDE.md          # User level
cat ./CLAUDE.md                   # Project level  
cat ./CLAUDE.local.md             # Local override (WINS)
```

---

## 3. 抽出タイミングが競合の窓を作る

**症状**: バックグラウンドの extractor がメモリを書き込むが、抽出が終わる前にユーザーが次のターンを始めてしまう。

**原因**: 抽出はレスポンスの末尾に走る。抽出が完了する前にユーザーがメッセージを送れてしまう。

**対策**: 競合する抽出リクエストはまとめる。カーソルは成功した実行の後にだけ進める。抽出に失敗した場合は、そのメッセージを次回あらためて再検討する。

---

## 4. 推論で導ける内容はメモリに入れない

**症状**: メモリインデックスが、すぐ古くなるアーキテクチャ詳細で埋まる。

**原因**: エージェントが、コードベースから導ける内容（アーキテクチャ、コードパターン、バージョン履歴）まで保存してしまう。

**対策**: 推論で導ける内容は設計上除外する。型の taxonomy で、すでに repo にあるものを保存できないようにする。

---

## 5. 並行実行の分類はツール単位ではなく呼び出し単位

**症状**: 「concurrent-safe」とされたツールが race condition を起こす。

**原因**: 同じツールでも、入力によって安全な場合と危険な場合がある。ツールの並行動作が固定だと思い込まない。

**対策**: 各呼び出しを実行時に分類する。

```typescript
// こうしない:
toolRegistry.register('shell', { concurrentSafe: false });

// こうする:
function isCallConcurrentSafe(call: ToolCall): boolean {
  if (call.args.command.startsWith('rm -rf')) return false;
  if (call.args.command.startsWith('cat')) return true;
  // ...runtime classification
}
```

---

## 6. 権限評価には副作用がある

**症状**: 権限チェックが、その後の呼び出しの挙動を変える。

**原因**: 権限 evaluator は拒否を追跡し、モードを変換し、副作用として state を更新する。純粋な lookup 関数ではない。

**対策**: 呼び出しをまたいで権限結果をキャッシュしない。毎回あらためて評価する。

---

## 7. ほとんどの async 作業は "pending" 状態を飛ばす

**症状**: UI には "pending" と表示されるのに、作業単位がその状態に入らない。

**原因**: 実際には、作業単位は直接 "running" として登録されることが多い。"pending" は state machine には存在するが、ほとんど使われない。

**対策**: すべての作業単位が pending から始まる前提で UI を作らない。

---

## 8. fork した子はさらに fork してはいけない

**症状**: context コストが指数関数的に膨らむ。

**原因**: 再帰的な fork で context が増殖する。親 + child1 + child2 + 孫...

**対策**: 1 段だけに制限する invariant を強制する。fork ツールは child の pool に残す（prompt cache 共有のため）が、呼び出し時にブロックする。

---

## 9. context builder は memoize されるが、手動で無効化する

**症状**: モデルがセッション全体で古いデータを見続ける。

**原因**: context builder は起動時にキャッシュされるが、mutation では cache が消えない。

**対策**: すべての mutation ポイントで、対応する cache を明示的に消す。

```typescript
// 例: mutation ポイントでの cache invalidation
async function editFile(path: string, content: string) {
  await writeFile(path, content);
  context.cache.invalidate(`file:${path}`); // MUST invalidate
}
```

---

## 10. hook の信頼判定は全体一括で行う

**症状**: 1 つの hook が untrusted なだけで、拡張システム全体が無効になる。

**原因**: workspace が untrusted だと、怪しいものだけでなく全ての hook がスキップされる。

**対策**: hook は dispatch 点で trust gate をかける設計にする。hook ごとの trust 評価はしない。

---

## 11. eviction には通知が必要

**症状**: 親が作業単位の結果を読めない。

**原因**: 親に完了通知が届く前に作業単位が evict される。race condition により、親はすでに GC 済みの結果を読もうとしてしまう。

**対策**: 2 段階で eviction する。
1. Clean disk output at terminal state (eager)
2. Clean in-memory record after parent notified (lazy)

---

## 12. skill 一覧の予算は厳しい

**症状**: skill の説明が途中で切れ、正しく trigger できない。

**原因**: skill の説明は連結され、エントリごとに上限（約 150 文字）がある。先頭に置いた trigger 用の文言が優先される。

**対策**: 特徴的な trigger 文言を先頭に置く。

```markdown
✓ Good: "harness-patterns: Memory, permissions, context engineering, multi-agent"
✗ Bad: "A comprehensive skill for understanding and implementing various patterns related to AI agent harnesses and runtime systems..."
```

---

## 13. tool のデフォルト権限は "Allow"

**症状**: tool が想定した gate を素通りする。

**原因**: 独自の権限ロジックを持たない tool は、完全に rule-based system に委ねられる。特に設定しない限り、デフォルトは "allow"。

**対策**: 機微な tool ではデフォルトを上書きする。

```typescript
registry.register('shell', {
  defaultPermission: 'ask', // NOT 'allow'
  // ...
});
```

---

## 14. team memory には auto-memory の有効化が必要

**症状**: 設定しているのに team 共有 memory が動かない。

**原因**: team memory は auto-memory と同じ directory / index 基盤の上に成り立っている。auto-memory を env var や settings で無効化すると、team memory も無効になる。

**対策**: team memory を有効にする前に、auto-memory が有効になっていることを確認する。feature gate と有効化チェックの両方を見る。

---

## 15. 孤立した topic ファイルがたまる

**症状**: ディスク容量が `.claude/memory/topics/` のファイルで埋まる。

**原因**: topic ファイルと index の 2 段階保存になっているため、途中で crash すると孤立した topic ファイルが残る。

**対策**: 定期的な sweep で、index から参照されていない topic ファイルを削除する。孤立ファイルは index を壊さないが、ディスク容量を消費する。

---

## 関連資料

- [Memory Persistence Pattern](memory-persistence-pattern.md) — Gotchas #1, #3, #4, #15
- [Tool Registry Pattern](tool-registry-pattern.md) — Gotchas #5, #6, #13
- [Multi-agent Pattern](multi-agent-pattern.md) — Gotchas #8, #11
- [Context Engineering Pattern](context-engineering-pattern.md) — Gotchas #9
- [Lifecycle Pattern](lifecycle-bootstrap-pattern.md) — Gotchas #10, #14
