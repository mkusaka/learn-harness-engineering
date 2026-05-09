# OpenAI 高度な標準作業手順書(SOP)

この標準作業手順書(SOP, Standard Operating Procedure)集は、原文ドキュメントの運用パターンをそのまま踏襲したり、応用したりできる具体的な実行指針書(playbook)に変換したものです。

SOPとは、繰り返し可能な作業を一貫して実行できるように段階的に整理した手順書であり、チームの暗黙知を明示的な知識へ変換するうえで重要な役割を果たします。

## 含まれる標準作業手順

- [`layered-domain-architecture.md`](./layered-domain-architecture.md):
  明示的な階層と横断的関心事(cross-cutting)の境界を設定する
- [`encode-knowledge-into-repo.md`](./encode-knowledge-into-repo.md):
  チャット・文書・メモリにある見えない知識をリポジトリのローカルファイルへ移す
- [`observability-feedback-loop.md`](./observability-feedback-loop.md):
  エージェントにログ・メトリクス・トレースと、繰り返し可能なデバッグループを提供する
- [`chrome-devtools-validation-loop.md`](./chrome-devtools-validation-loop.md):
  ブラウザ自動化とスナップショットを活用して、UI の動作が十分に整うまで検証する

## 使用方法

1. 現在のボトルネックに合う標準作業手順を選んでください。
2. チェックリストを使って、抜けている成果物やツールを整えてください。
3. その結果得られたルールを、コピーした `repo-template/` のドキュメントにエンコードしてください。
4. 繰り返し出るレビュー指摘は、検査・スクリプト・ガードレール(guardrail)に置き換えてください。

これらの手順を盲目的に従う必要はありません。これらはハーネス(harness)をより明確にし、強制可能で、繰り返し可能にするためのものです。
