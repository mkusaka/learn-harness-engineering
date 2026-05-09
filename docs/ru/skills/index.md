# Skills

このディレクトリには、コースに付属する AI エージェント向けの組み込み skill が含まれています。skill は、特定のタスクを実行するために AI エージェント（Claude Code, Codex, Cursor, Windsurf など）が読み込める、自己完結したプロンプトテンプレートです。

## harness-creator

AI エージェント向けの harness engineering における production-grade な skill です。harness の 5 つの主要サブシステムである、instructions、state、verification、scope、session lifecycle を作成・評価・改善するのに役立ちます。

### できること

- **harness をゼロから作成する** — AGENTS.md、機能一覧、検証フロー
- **既存の harness を改善する** — 5 つのサブシステムに基づく評価と、優先度付きの改善提案
- **セッションの継続性を設計する** — メモリの永続化、進捗追跡、handoff 手順
- **production パターンを適用する** — メモリ、context engineering、ツールの安全性、マルチエージェントの連携

### クイックスタート

skill のファイルは、リポジトリ内の [`skills/harness-creator/`](https://github.com/walkinglabs/learn-harness-engineering/tree/main/skills/harness-creator) にあります。

Claude Code で使うには、`harness-creator/` ディレクトリをプロジェクトの skill 配置先にコピーするか、エージェントに `SKILL.md` ファイルを指定してください。

### 参考パターン

この skill には、6 つの詳細な参考ドキュメントが含まれています。

| パターン | 使いどき |
|---------|-------------|
| Memory Persistence | エージェントがセッション間で内容を忘れるとき |
| Context Engineering | コンテキスト予算の管理、JIT ロード |
| Tool Registry | ツールの安全性、並行制御 |
| Multi-Agent Coordination | 並列処理、専門化されたフロー |
| Lifecycle & Bootstrap | フック、バックグラウンドタスク、初期化 |
| Gotchas | 修正付きの、見落としやすい 15 の失敗モード |

### テンプレート

この skill には、すぐ使えるテンプレートが含まれています。

- `agents.md` — 実用ルールを含む AGENTS.md のひな形
- `feature-list.json` — JSON Schema と機能一覧のサンプル
- `init.sh` — 標準的な初期化スクリプト
- `progress.md` — セッション進捗ログのテンプレート

### この skill の作り方

`harness-creator` は、エージェント skill の作成・テスト・反復のための Anthropic 公式のメタ skill である **skill-creator** の手法を使って開発されました。skill-creator は、組み込みの eval runner、grader、ベンチマーク閲覧ツールを備えた、構造化されたワークフロー（draft → test → evaluate → iterate）を提供します。

- **skill-creator の元資料**: [anthropics/skills — skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator)
- **Claude Code の skill ドキュメント**: [anthropics/claude-code — plugin-dev/skills](https://github.com/anthropics/claude-code/tree/main/plugins/plugin-dev/skills)
