# OpenAI Advanced SOPs

これらの SOP は、記事にある運用パターンを、実際に従ったり
応用したりできる具体的な実行用プレイブックへと落とし込んだものです。

## 含まれる SOP

- [`layered-domain-architecture.md`](./layered-domain-architecture.md):
  明示的なレイヤーと横断的な境界を定義する
- [`encode-knowledge-into-repo.md`](./encode-knowledge-into-repo.md):
  チャット、ドキュメント、記憶の中にある見えない知識を repo ローカルのファイルへ移す
- [`observability-feedback-loop.md`](./observability-feedback-loop.md):
  エージェントにログ、メトリクス、トレース、そして再現可能なデバッグループを与える
- [`chrome-devtools-validation-loop.md`](./chrome-devtools-validation-loop.md):
  ブラウザ自動化とスナップショットを使って、UI の挙動が問題なくなるまで検証する

## 使い方

1. 現在のボトルネックに合う SOP を選びます。
2. チェックリストを使って、不足している成果物やツールを整えます。
3. その結果得られたルールを、コピーした `repo-template/` のドキュメントに反映します。
4. 繰り返し出るレビューコメントを、チェック、スクリプト、ガードレールに変換します。

これらは盲目的に従うためのものではありません。ハーネスをより
読みやすく、強制しやすく、再現しやすくするためのものです。
