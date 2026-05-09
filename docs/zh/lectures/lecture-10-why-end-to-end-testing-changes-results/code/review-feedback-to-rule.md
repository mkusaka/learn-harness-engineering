# 例: レビュー指摘をルールに落とし込む

繰り返し受けるレビューコメント:

> Do not call filesystem utilities from the renderer. Use the preload bridge.

昇格させたハーネスルール:

- renderer コードでの `fs` の使用を防ぐ lint ルールまたは import ルールを追加する
- preload の境界を説明する是正用の説明文を追加する
