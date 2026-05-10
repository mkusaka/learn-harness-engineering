# Skills

このディレクトリには、このコースに同梱されている AI エージェント用の skills が含まれています。Skills は自己完結したプロンプトテンプレートであり、AI コーディングエージェント（Claude Code、Codex、Cursor、Windsurf など）に読み込ませて、特定の作業を実行するために使えます。

## harness-creator

AI コーディングエージェント向けの、本番運用レベルの harness engineering skill です。instructions、state、verification、scope、session lifecycle という 5 つの中核サブシステムを作成・評価・改善するのに役立ちます。

### できること

- **harness をゼロから作成する** — AGENTS.md、機能一覧、検証ワークフロー
- **既存の harness を改善する** — 5 サブシステムを評価し、優先度付きで改善する
- **セッションの継続性を設計する** — メモリの永続化、進捗追跡、引き継ぎ手順
- **本番向けパターンを適用する** — メモリ、コンテキストエンジニアリング、ツール安全性、マルチエージェント協調

### クイックスタート

skill ファイルはリポジトリ内の [`skills/harness-creator/`](https://github.com/walkinglabs/learn-harness-engineering/tree/main/skills/harness-creator) にあります。

Claude Code で使うには、`harness-creator/` ディレクトリをプロジェクトの skill パスにコピーするか、エージェントに SKILL.md ファイルを参照させてください。

### 参照パターン

この skill には、6 つの詳細な参照ドキュメントが含まれています。

| Pattern | When to Use |
|---------|-------------|
| Memory Persistence | セッション間でエージェントが記憶を失う場合 |
| Context Engineering | コンテキスト予算の管理、JIT ロード |
| Tool Registry | ツールの安全性、同時実行制御 |
| Multi-Agent Coordination | 並列処理、専門化したワークフロー |
| Lifecycle & Bootstrap | フック、バックグラウンドタスク、初期化 |
| Gotchas | 目立ちにくい 15 の失敗モードと対策 |

### テンプレート

この skill には、すぐに使えるテンプレートが同梱されています。

- `agents.md` — 実運用ルールを備えた AGENTS.md のひな形
- `feature-list.json` — JSON Schema + 機能一覧の例
- `init.sh` — 標準初期化スクリプト
- `progress.md` — セッション進捗ログのテンプレート

### この Skill の作り方

`harness-creator` は、**skill-creator** 手法を使って開発されました。これは、エージェント skill を作成・テスト・反復改善するための Anthropic 公式のメタ skill です。skill-creator は、組み込みの eval 実行器、採点器、ベンチマークビューアを備えた、構造化されたワークフロー（draft → test → evaluate → iterate）を提供します。

- **skill-creator source**: [anthropics/skills — skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator)
- **Claude Code skills docs**: [anthropics/claude-code — plugin-dev/skills](https://github.com/anthropics/claude-code/tree/main/plugins/plugin-dev/skills)
