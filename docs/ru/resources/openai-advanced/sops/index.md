# OpenAI Advanced SOPs

これらの SOP は、記事にある運用パターンを、実際に実行できる具体的なプレイブックへと落とし込みます。必要に応じて、そのまま使うことも、状況に合わせて調整することもできます。

## 含まれる SOP

- [`layered-domain-architecture.md`](./layered-domain-architecture.md):
  明示的なレイヤーと横断的な境界を導入する
- [`encode-knowledge-into-repo.md`](./encode-knowledge-into-repo.md):
  チャット、ドキュメント、記憶の中にある見えない知識を、リポジトリ内のファイルへ移す
- [`observability-feedback-loop.md`](./observability-feedback-loop.md):
  エージェントにログ、メトリクス、トレース、そして再現可能なデバッグループを与える
- [`chrome-devtools-validation-loop.md`](./chrome-devtools-validation-loop.md):
  ブラウザ自動化とスクリーンショットを使って、UI の挙動をクリーンな結果になるまで検証する

## 使い方

1. 今のボトルネックに合う SOP を選びます。
2. チェックリストを使って、不足している成果物やツールを整えます。
3. 得られたルールを、コピーした `repo-template/` のドキュメントに反映します。
4. 繰り返し出るレビューコメントは、チェックやスクリプト、ガードレールに置き換えます。

これは盲目的に従うためのものではありません。harness をより読みやすく、検証しやすく、再現しやすくするためのものです。
