[中国語版 →](../../../zh/projects/project-04-incremental-indexing/)

> 関連講義: [Lecture 07. エージェントのタスク境界を明確に定める](./../../lectures/lecture-07-why-agents-overreach-and-under-finish/index.md) · [Lecture 08. feature list を使ってエージェントの行動を制限する](./../../lectures/lecture-08-why-feature-lists-are-harness-primitives/index.md)
> テンプレートファイル: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/en/resources/templates/)

# プロジェクト 04. runtime フィードバックでエージェントの挙動を調整する

## 何をするか

runtime の可観測性（起動ログ、インポート/インデックス作成ログ、エラー状態）と、レイヤー境界の逸脱を防ぐためのアーキテクチャ制約を追加します。エージェントに runtime のバグを渡して、修正させてください。

課題は2回実行します。1回目はログも制約もない状態で実行し、2回目は適切なツールとルールを用意した状態で実行します。

## ツール

- Claude Code または Codex
- Git
- Node.js + Electron

## harness の仕組み

runtime フィードバック + スコープ制御 + インクリメンタルなインデックス作成
