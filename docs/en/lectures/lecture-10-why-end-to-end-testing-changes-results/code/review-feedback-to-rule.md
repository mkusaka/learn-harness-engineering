# 例: レビューコメントをルールに変える

繰り返し出たレビューコメント:

> renderer から filesystem ユーティリティを呼び出さないでください。preload bridge を使ってください。

ハーネスのルールに昇格した内容:

- `fs` の使用を renderer code で禁止する lint または import ルールを追加する
- preload boundary を説明する remediation text を追加する
