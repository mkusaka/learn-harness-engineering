# メモリと永続化のパターン

## Problem

永続メモリがなければ、セッションが終わった瞬間にエージェントはユーザーの好み、プロジェクトの文脈、行動に対するフィードバックをすべて失います。ユーザーは毎回同じ修正を繰り返し伝えなければならず（「`npm` ではなく `bun` を使って」など）、エージェントも、時間をかけて本当に役立つようになるための実践知を蓄積できません。

## Golden Rules

### スコープと持続性で層を分ける

- **Instruction memory**（人間が管理し、バージョン管理されるもの）: AGENTS.md, CLAUDE.md, project conventions
- **Auto-memory**（エージェントが書き込み、永続化されるもの）: Progress logs, session handoffs, discovered patterns
- **Session extraction**（バックグラウンドで導出されるもの）: セッション終了時の自動トランスクリプト解析

### 2段階保存の不変条件

すべてのメモリ書き込みは、次の2段階で行います。
1. 専用の topic file に完全な内容を書き込む
2. index に1行のポインタを追記する

この2つの間で処理がクラッシュしても、最悪の結果は孤立した topic file が1つ残るだけです。index の整合性は保たれます。

### Local override が常に勝つ

同じトピックが複数のスコープで扱われる場合は、最もローカルな指示が優先されます。

```
Organization-wide → User-level → Project-level → Local override
     ↓                  ↓              ↓              ↓
   下限を定める      そこから絞る    さらに絞る        最終決定
```

### index は上限付きの常時コンテキスト、topic file は必要時に読む詳細情報

- **Index**: 約200行 / 25KB を上限とし、1エントリ1行で管理する
- **Topic files**: 詳細量に上限はなく、必要に応じて読み込む

## 使用する場面

- エージェントがセッションをまたいで持続し、ユーザーの好みやプロジェクトの文脈を覚えておく必要がある
- 複数の指示スコープが共存していて、明確な優先順位が必要である
- 手動の整理なしに、セッションから学習してほしい
- ユーザーを待たせないバックグラウンド抽出が必要である

## トレードオフ

| Decision | Benefit | Cost |
|---|---|---|
| Layered memory | 各スコープを独立に共有、監査、上書きできる | 起動時に見つけるべきファイルが増える |
| Local-wins priority | 共有ファイルを触らずにユーザーが上書きできる | グローバルルールが静かに上書きされうる |
| Bounded index with on-demand topics | メモリ量に関係なくコンテキストコストが一定 | エージェントが追加の検索ステップを実行する必要がある |
| Background extraction | ユーザー応答に待ち時間を追加しない | 抽出と次のターンの間に競合ウィンドウがある |

## 実装パターン

1. 起動時に冪等に **memory directory を定義する**（例: `.claude/memory/`）
2. 読み込み時にハード上限を強制する **index file を作成する**
3. **2段階保存を実装する**: まず topic file、その後に index を更新する
4. すべての tool call が終わった最終応答の後にだけ **background extraction を起動する**
5. **相互排他を強制する**: メインエージェントが memory に書き込んだ turn では抽出をスキップする
6. レイヤー横断の昇格提案に対する **review mechanism を構築する**

## 注意点

1. **index の切り詰めは、発生するまで静かに進む** — エントリは短く保つ
2. **優先順位の並びは直感に反しやすい** — local が project より、project が user より、user が org より強い
3. **抽出のタイミングには競合ウィンドウがある** — 抽出が終わる前にユーザーが次の turn を始められる
4. **導出できる内容は memory に置かない** — アーキテクチャやコードパターンは codebase から再導出できる
5. **孤立した topic file が蓄積する** — 定期的な cleanup を推奨する

## 関連パターン

- [Context Engineering](context-engineering-pattern.md) — 各層にまたがるコンテキスト予算の管理方法
- [Lifecycle & Bootstrap](lifecycle-bootstrap-pattern.md) — 初期化時に memory をどう読み込むか

## テンプレート: Progress Log の構成

```markdown
# Session Progress Log

## Current State (Last Updated: YYYY-MM-DD HH:MM)

**Active Feature:** feat-003 - Q&A with Citations
**Status:** In Progress (60% complete)

### What's Done
- [x] Document chunking pipeline
- [x] Index data structure
- [ ] Q&A handler (in progress)

### What's In Progress
- Implementing Q&A IPC handler
- Need to decide: streaming vs batch response

### Blockers
- Waiting on decision: citation format (footnotes vs inline)

### Next Session Should
1. Complete Q&A handler
2. Add citation formatting
3. Test end-to-end flow
```

## 根拠

このパターンは、Claude Code の memory system を含む本番の agent runtime に基づいています。そこでは次の仕組みが実装されています。
- 4段階の instruction hierarchy（org/user/project/local）
- 4種類の auto-memory taxonomy（user/feedback/project/reference）
- 相互排他を伴う background session extraction
- 拡張レイヤーとしての team-shared memory
