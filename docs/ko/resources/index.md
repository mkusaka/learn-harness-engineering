# 日本語リソースライブラリ (Resource Library)

このフォルダは、講義で紹介したメソッド(method)を、実際のリポジトリにそのままコピーして使えるテンプレート(template)と、簡潔な参考資料としてまとめたものです。エージェント(agent)が複数セッションにわたって一貫して動作するには、毎回設定・状態・範囲を個別に再導出するのではなく、これらの資料を土台にする必要があります。

## いつ使うか

Codex、Claude Code、またはほかのコーディングエージェントが複数セッションにわたって作業するときの出発点として使ってください。

特に、次のような状況で役立ちます。

- 作業が複数セッションにまたがるとき
- 機能(feature)が多く、途中のまま放置されやすいとき
- エージェントが早すぎる段階で完了を宣言しがちなとき
- 開始手順を毎回あらためて見つける必要があるとき

## 始め方

最小構成にする場合は、まず次のファイルから始めてください。

- ルート指示: [`templates/AGENTS.md`](./templates/AGENTS.md) または [`templates/CLAUDE.md`](./templates/CLAUDE.md)
- 機能状態: [`templates/feature_list.json`](./templates/feature_list.json)
- 進行ログ: [`templates/claude-progress.md`](./templates/claude-progress.md)
- ブートストラップ(bootstrap)スクリプトの参照: `docs/ko/resources/templates/init.sh`

そのあと、次のファイルを追加してください。

- セッションの引き継ぎ(session handoff): [`templates/session-handoff.md`](./templates/session-handoff.md)
- クリーン状態(clean state)チェックリスト: [`templates/clean-state-checklist.md`](./templates/clean-state-checklist.md)
- 評価者(evaluator)ルーブリック(rubric): [`templates/evaluator-rubric.md`](./templates/evaluator-rubric.md)

"Harness engineering" の投稿に出てくる OpenAI スタイルの完全なリポジトリ構造が必要なら、高度版パックを使ってください。

- [`openai-advanced/index.md`](./openai-advanced/index.md)

## ライブラリ構成

- [`templates/`](./templates/index.md): 実際のリポジトリにコピーできるテンプレート
- [`reference/`](./reference/index.md): メソッドノート、開始フロー、失敗タイプマップ
- [`openai-advanced/`](./openai-advanced/index.md): 高度なリポジトリスケルトン(skeleton)、システム・オブ・レコード(SoR)文書、エージェント優先ガバナンステンプレート

## 推奨最小パック

- `AGENTS.md` または `CLAUDE.md`
- `feature_list.json`
- `claude-progress.md`
- `init.sh`

この4ファイルだけでも、ほとんどのエージェントワークフローを目に見えて安定させられます。

リポジトリが、複数ドメイン、アクティブな計画(plan)、品質スコア、安定性ポリシーを備えた長期システムへと発展したら、最小パックをむやみに増やすのではなく、[`openai-advanced/`](./openai-advanced/index.md) パックへ移行してください。
