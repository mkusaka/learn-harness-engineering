# リソースライブラリ

このフォルダには、コースの手法をそのまま使えるテンプレートと、実際のリポジトリで使いやすい簡潔なリファレンスにまとめています。

## 使うタイミング

Codex、Claude Code、または他のコードエージェントに、セットアップ、状態、スコープを毎回書き直させず、複数セッションにわたって作業させたいときは、ここから始めてください。

特に次のような場合に有効です。

- 作業が複数セッションにまたがる
- 機能が多く、未完了のまま残りやすい
- エージェントが早すぎる段階で完了宣言しがち
- 開始手順を毎回ゼロから確認している

## まずここから

最小構成にするなら、次から始めてください。

- ルート用の指示: [`templates/AGENTS.md`](./templates/AGENTS.md) または [`templates/CLAUDE.md`](./templates/CLAUDE.md)
- 機能の状態: [`templates/feature_list.json`](./templates/feature_list.json)
- 進捗ログ: [`templates/claude-progress.md`](./templates/claude-progress.md)
- bootstrap スクリプトの参照: `docs/en/resources/templates/init.sh`

そのあとに追加します。

- session handoff: [`templates/session-handoff.md`](./templates/session-handoff.md)
- クリーン終了チェックリスト: [`templates/clean-state-checklist.md`](./templates/clean-state-checklist.md)
- evaluator 用ルーブリック: [`templates/evaluator-rubric.md`](./templates/evaluator-rubric.md)

「Harness engineering」の投稿で紹介されている OpenAI 風のより完全なリポジトリ構成が必要なら、advanced パックを使ってください。

- [`openai-advanced/index.md`](./openai-advanced/index.md)

## ライブラリ構成

- [`templates/`](./templates/index.md): 実際のリポジトリにコピーするためのテンプレート
- [`reference/`](./reference/index.md): 手法、開始フロー、失敗モードのマップに関するメモ
- [`openai-advanced/`](./openai-advanced/index.md): 高度なリポジトリの骨組み、system-of-record ドキュメント、agent-first 管理のテンプレート

## 推奨最小パック

- `AGENTS.md` または `CLAUDE.md`
- `feature_list.json`
- `claude-progress.md`
- `init.sh`

この4ファイルがあれば、ほとんどのエージェントワークフローをかなり安定させられます。

リポジトリが、複数ドメイン・アクティブな計画・品質評価・信頼性ポリシーを持つ長寿命システムへ成長したら、最小パックを無理に引き延ばすのではなく、[`openai-advanced/`](./openai-advanced/index.md) のパックに移行してください。
