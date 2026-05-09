[英語版 →](../../../../en/resources/openai-advanced/sops/) | [中国語版 →](../../../../zh/resources/openai-advanced/sops/)

# SOP ライブラリ

harness をセットアップし、運用するための段階的な標準手順をまとめたものです。

## 利用可能な SOP

- [`layered-domain-architecture.md`](./layered-domain-architecture.md):
  エージェントが境界を越えないように、階層化されたドメインアーキテクチャを構築します
- [`encode-knowledge-into-repo.md`](./encode-knowledge-into-repo.md):
  チャット、ドキュメント、記憶にある暗黙知を、リポジトリ内のローカルファイルへ移します
- [`observability-feedback-loop.md`](./observability-feedback-loop.md):
  エージェントに log、metrics、trace、そして再現可能なデバッグループを提供します
- [`chrome-devtools-validation-loop.md`](./chrome-devtools-validation-loop.md):
  browser 自動化と snapshot を使って、UI の挙動が期待どおりになるまで検証します

## 使い方

1. 現在のボトルネックに合う SOP を選びます。
2. チェックリストを使って、足りない artifact や tool を整えます。
3. 得られたルールを、コピー済みの `repo-template/` 配下の文書に反映します。
4. 何度も出る review コメントを、check、script、または guardrail に置き換えます。

これらは盲目的に従うためのものではありません。harness をより読みやすく、実行可能で、再現しやすいものにするために設計されています。
