[英語版 →](../../../en/projects/project-04-incremental-indexing/)

> 関連講義: [講義 07. エージェントのために明確な作業境界を設定する](./../../lectures/lecture-07-why-agents-overreach-and-under-finish/index.md) · [講義 08. 機能一覧を使ってエージェントの作業を制限する](./../../lectures/lecture-08-why-feature-lists-are-harness-primitives/index.md)
> テンプレートファイル: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/en/resources/templates/)

# プロジェクト 04. ランタイムフィードバックを使ってエージェントの動作を修正する

## やること

ランタイムの観測可能性（runtime observability、起動ログ、インポート/インデックスログ、エラー状態）を追加し、レイヤー間違反（cross-layer violation）を防ぐためのアーキテクチャ制約（architecture constraints）を導入します。エージェントが修正できるよう、あえてランタイムバグを仕込んでおきます。

エージェントは、ログ（log）やエラー出力がなければ内部エラーを認識しにくく、存在しない機能まで「実装済み」だと誤って判断することがあります。ランタイムのフィードバックループがあれば、エージェントは実行結果を直接観察し、その場で動作を修正できます。

2 回実行します。1 回目はログや制約なしで進め、2 回目は適切なツールとルール（rules）を備えた状態で進めます。

## ツール

- Claude Code または Codex
- Git
- Node.js + Electron

## ハーネスメカニズム

ランタイムフィードバック + 範囲制御 + 増分インデックス作成（incremental indexing）
