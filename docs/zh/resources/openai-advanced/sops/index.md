# OpenAI 高度な SOP

これらの SOP は、記事内の作業方法を、そのまま参照・実行・改変できる操作手順に落とし込んだものです。

## 含まれる SOP

- [`layered-domain-architecture.md`](./layered-domain-architecture.md)：
  明示的な階層構造と、ドメインをまたぐ境界を構築する
- [`encode-knowledge-into-repo.md`](./encode-knowledge-into-repo.md)：
  会話、外部ドキュメント、頭の中の知識を repo-local な文書に変える
- [`observability-feedback-loop.md`](./observability-feedback-loop.md)：
  agent に logs、metrics、traces と再現可能なデバッグループを提供する
- [`chrome-devtools-validation-loop.md`](./chrome-devtools-validation-loop.md)：
  ブラウザ自動化とスナップショットで UI を繰り返し検証し、clean になるまで続ける

## 使い方

1. まず、今のボトルネックに最も合う SOP を選びます。
2. チェックリストに従って、不足している成果物やツールを補います。
3. 得られたルールを `repo-template/` 内の文書に再びコード化します。
4. 何度も出てくる review コメントは、チェック、スクリプト、または guardrail に昇格させます。

これらの SOP は機械的に書き写すためのものではなく、harness をより読みやすく、より実行しやすく、より再利用しやすくするためのものです。
