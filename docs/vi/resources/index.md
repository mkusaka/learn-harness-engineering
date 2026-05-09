# リソースライブラリ

このディレクトリは、コースの手法を、そのままコピーして使えるテンプレートと、実際のリポジトリで参照できる簡潔な資料にまとめたものです。

## いつ使うか

Codex、Claude Code、または別の coding agent を、設定・状態・スコープを何度も立て直さずに複数セッションにわたって動かしたいときは、ここから始めてください。

次のような場合に特に役立ちます。

- 作業が複数セッションにまたがる
- 機能が多く、途中で中断されやすい
- agent が成功を早めに宣言しがち
- 初期化手順を毎回最初から探し直している

## ここから始める

最小構成なら、まず次を使ってください。

- 基本ガイド: [`templates/AGENTS.md`](./templates/AGENTS.md) または [`templates/CLAUDE.md`](./templates/CLAUDE.md)
- 機能状態: [`templates/feature_list.json`](./templates/feature_list.json)
- 進捗ログ: [`templates/claude-progress.md`](./templates/claude-progress.md)
- 起動スクリプト参照: `docs/vi/resources/templates/init.sh`

その後、必要に応じて次を追加します。

- セッション引き継ぎ: [`templates/session-handoff.md`](./templates/session-handoff.md)
- クリーン終了チェックリスト: [`templates/clean-state-checklist.md`](./templates/clean-state-checklist.md)
- 評価基準: [`templates/evaluator-rubric.md`](./templates/evaluator-rubric.md)

記事「Harness engineering」をベースに、OpenAI 方式のリポジトリ構成をより完全に取り入れたい場合は、拡張版を使ってください。

- [`openai-advanced/index.md`](./openai-advanced/index.md)

## ライブラリ構成

- [`templates/`](./templates/index.md): 実際のリポジトリにコピーするためのテンプレート
- [`reference/`](./reference/index.md): 手法メモ、起動フロー、障害モードの図解
- [`openai-advanced/`](./openai-advanced/index.md): 拡張リポジトリの枠組み、単一の信頼できる情報源となる資料、agent 優先の運用テンプレート

## 推奨の最小パッケージ

- `AGENTS.md` または `CLAUDE.md`
- `feature_list.json`
- `claude-progress.md`
- `init.sh`

この 4 つのファイルがあれば、ほとんどの agent ワークフローを十分に安定させられます。

リポジトリが、複数ドメイン、運用計画、品質採点、信頼性ポリシーを備えた、より長期稼働のシステムへ成長したら、最小パッケージを無理に拡張するのではなく、[`openai-advanced/`](./openai-advanced/index.md) に移行してください。
