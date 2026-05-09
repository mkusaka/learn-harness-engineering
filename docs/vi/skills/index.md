# スキル (Skills)

このディレクトリには、このコースに付属するスキル集が含まれています。スキルは、AI コーディングエージェント（Claude Code、Codex、Cursor、Windsurf など）が専門的なタスクを実行するために読み込める、独立したプロンプトテンプレートです。

## harness-creator

AI コーディングエージェント向けの実運用レベル（production-grade）のハーネス構築スキルです。ハーネスの5つの中核システムである、指示（instructions）、状態（state）、検証（verification）、スコープ（scope）、セッションライフサイクル（session lifecycle）を作成し、評価し、改善するのに役立ちます。

### 機能

- **ハーネスをゼロから作成する** — AGENTS.md、機能一覧、検証ワークフロー
- **既存のハーネスを改善する** — 優先度付きの改善を伴う5つのサブシステムを評価する
- **セッションの継続性を設計する** — メモリ保存、進捗追跡、引き継ぎ手順
- **production パターンを適用する** — メモリ、コンテキスト技法、ツールの安全性、マルチエージェント調整

### クイックスタート

スキルファイルはリポジトリ内の [`skills/harness-creator/`](https://github.com/walkinglabs/learn-harness-engineering/tree/main/skills/harness-creator) にあります。

Claude Code で使うには、`harness-creator/` ディレクトリをプロジェクトのスキルパスへコピーするか、エージェントに SKILL.md ファイルを指定してください。

### 参照パターン

このスキルには、6つの詳細な参照ドキュメントが含まれています。

| パターン | 使用する場面 |
|---------|-------------|
| メモリ保存 | エージェントがセッション間で忘れてしまう |
| コンテキスト技法 | コンテキスト予算の管理、JIT ロード |
| ツール登録 | ツールの安全性、同時実行制御 |
| マルチエージェント調整 | 並列処理、専門化されたワークフロー |
| ライフサイクル & 起動 | フック、バックグラウンドタスク、初期化 |
| 落とし穴 (Gotchas) | 15 の不明瞭な失敗モードとその修正方法 |

### テンプレート

このスキルには、すぐに使えるテンプレートが含まれています。

- `agents.md` — 運用ルールを含む AGENTS.md のひな形
- `feature-list.json` — JSON Schema + 機能一覧のサンプル
- `init.sh` — 標準的な初期化スクリプト
- `progress.md` — セッション進捗ログのテンプレート

### このスキルの作成方法

`harness-creator` は、Anthropic の公式メタスキルである **skill-creator** を使って開発されました。skill-creator は、エージェント向けスキルを作成し、テストし、反復的に改善するための構造化されたワークフロー（ドラフト → テスト → 評価 → 反復）を提供し、評価ランナー（eval runners）、採点器（graders）、ベンチマークビューア（benchmark viewer）を標準で組み込んでいます。

- **skill-creator のソース**: [anthropics/skills — skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator)
- **Claude Code のスキル関連ドキュメント**: [anthropics/claude-code — plugin-dev/skills](https://github.com/anthropics/claude-code/tree/main/plugins/plugin-dev/skills)
