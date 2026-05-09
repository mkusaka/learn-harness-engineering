# Skills（技能集）

このディレクトリには、コースに付属する AI agent 向けの skills が含まれています。各 skill は自己完結したプロンプトテンプレートで、AI プログラミングエージェント（Claude Code、Codex、Cursor、Windsurf など）が読み込んで専門的なタスクを実行できます。

## harness-creator

AI プログラミングエージェント向けの、本番運用を想定した harness エンジニアリング skill です。指示、状態、検証、スコープ、セッションライフサイクルという 5 つの中核サブシステムの作成、評価、改善を支援します。

### 它能做什么

- **harness をゼロから作成** — AGENTS.md、機能一覧、検証ワークフロー
- **既存の harness を改善** — 5 サブシステムの採点と、優先順位付きの改善提案
- **セッションの継続性を設計** — 記憶の永続化、進捗追跡、引き継ぎの仕組み
- **本番向けパターンを適用** — 記憶、コンテキストエンジニアリング、ツールの安全性、マルチエージェント協調

### 快速开始

skill ファイルは、リポジトリ内の [`skills/harness-creator/`](https://github.com/walkinglabs/learn-harness-engineering/tree/main/skills/harness-creator) ディレクトリにあります。

Claude Code で使う場合は、`harness-creator/` ディレクトリをプロジェクトの skill パスにコピーするか、agent に `SKILL.md` を直接読ませてください。

### 参考模式

この skill には、6 つの詳細なパターン参考ドキュメントが含まれています。

| 模式 | 适用场景 |
|------|----------|
| 記憶の永続化 | agent がセッションをまたいでプロジェクト知識を忘れる場合 |
| コンテキストエンジニアリング | コンテキスト予算の管理、オンデマンド読み込み、委譲の分離 |
| ツール登録 | ツールの安全性、並行制御、権限制御のパイプライン |
| マルチエージェント協調 | 並列化、専門化、調査者→実装者のワークフロー |
| ライフサイクルと起動 | フック、バックグラウンドタスク、初期化シーケンス |
| よくある落とし穴 | 15 個の失敗パターンと修正方法 |

### 模板

この skill には、すぐに使えるテンプレートも付属しています。

- `agents.md` — 作業ルールを含む AGENTS.md のひな形
- `feature-list.json` — JSON Schema + 功能列表示例
- `init.sh` — 標準の初期化スクリプト
- `progress.md` — セッション進捗ログのテンプレート

### 开发过程

`harness-creator` は **skill-creator** の方法論に基づいて開発されています。これは Anthropic が提供するメタ skill で、agent skill の作成、テスト、反復改善に使います。skill-creator には、下書き → テスト → 評価 → 反復 という構造化されたワークフローがあり、評価ランナー、スコアラー、ベンチマークビューアが組み込まれています。

- **skill-creator の出典**：[anthropics/skills — skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator)
- **Claude Code skill ドキュメント**：[anthropics/claude-code — plugin-dev/skills](https://github.com/anthropics/claude-code/tree/main/plugins/plugin-dev/skills)
