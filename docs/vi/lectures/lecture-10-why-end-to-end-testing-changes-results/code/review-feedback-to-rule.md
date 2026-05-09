# 例: レビューのフィードバックをルールに変える

繰り返し出てくるレビューコメント:

> renderer から filesystem utilities を呼び出さないこと。preload bridge を使うこと。

ハーネスのルールに反映すると:

- renderer コードで `fs` の使用を防ぐ lint ルールまたは import ルールを追加する
- preload の境界を説明する解決策の文書を追加する
