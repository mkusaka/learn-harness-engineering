# スキル (Skills)

このディレクトリには、本講座に付属するバンドル済みの AI エージェント用スキルが含まれています。スキルは、AI コーディングエージェント（Claude Code, Codex, Cursor, Windsurf など）が特定の作業を行うために読み込める、自己完結型のプロンプトテンプレートです。

## harness-creator

AI コーディングエージェント向けの、プロダクションレベルのハーネスエンジニアリング用スキルです。5 つの中核となるハーネスのサブシステムである、指示、状態、検証、範囲、セッションライフサイクルを生成・評価・改善するのに役立ちます。

### 機能 (What It Does)

- **ゼロからハーネスを作成** — AGENTS.md、機能一覧、検証ワークフロー
- **既存のハーネスを改善** — 5 つのサブシステム評価と、優先順位付きの改善項目
- **セッションの継続性を設計** — メモリの永続化、進捗追跡、ハンドオフ手順
- **本番向けパターンを適用** — メモリ、コンテキストエンジニアリング、ツール安全性、マルチエージェント調整

### クイックスタート (Quick Start)

スキルファイルは、リポジトリの [`skills/harness-creator/`](https://github.com/walkinglabs/learn-harness-engineering/tree/main/skills/harness-creator) にあります。

Claude Code で使う場合は、`harness-creator/` ディレクトリをプロジェクトのスキルパスにコピーするか、エージェントが SKILL.md ファイルを参照するように設定してください。

### 参考パターン (Reference Patterns)

このスキルには、6 つの詳細な参考ドキュメントが含まれています。

| パターン (Pattern) | 使用時期 (When to Use) |
|---------|-------------|
| Memory Persistence | エージェントがセッションをまたいで記憶を失うとき |
| Context Engineering | コンテキスト予算の管理、JIT ロード |
| Tool Registry | ツールの安全性、同時実行制御 |
| Multi-Agent Coordination | 並列化、特化ワークフロー |
| Lifecycle & Bootstrap | フック、バックグラウンド作業、初期化 |
| Gotchas | 15 の非自明な失敗パターンと修正方法 |

### テンプレート (Templates)

このスキルには、すぐに使えるテンプレートが含まれています。

- `agents.md` — 作業ルールを含む AGENTS.md のスキャフォールド
- `feature-list.json` — JSON Schema と機能一覧の例
- `init.sh` — 標準的な初期化スクリプト
- `progress.md` — セッション進捗ログ用テンプレート

### このスキルの作成方法 (How This Skill Was Built)

`harness-creator` は **skill-creator** の方法論を使って開発されました。これは、エージェント用スキルを作成・テスト・反復するための Anthropic 公式のメタスキルです。skill-creator には、構造化されたワークフロー（draft → test → evaluate → iterate）と、組み込みの評価ランナー（eval runner）、採点器（grader）、ベンチマークビューア（benchmark viewer）が用意されています。

- **skill-creator ソース**: [anthropics/skills — skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator)
- **Claude Code スキルドキュメント**: [anthropics/claude-code — plugin-dev/skills](https://github.com/anthropics/claude-code/tree/main/plugins/plugin-dev/skills)
