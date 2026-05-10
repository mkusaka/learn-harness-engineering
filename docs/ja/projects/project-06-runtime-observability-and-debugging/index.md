[英語版 →](../../../en/projects/project-06-runtime-observability-and-debugging/) | [中国語版 →](../../../zh/projects/project-06-runtime-observability-and-debugging/)

> 関連講義: [Lecture 11. エージェントの実行時状態を可観測にする](./../../lectures/lecture-11-why-observability-belongs-inside-the-harness/index.md) · [Lecture 12. すべてのセッションの最後にクリーンな引き継ぎを行う](./../../lectures/lecture-12-why-every-session-must-leave-a-clean-state/index.md)
> テンプレート: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/ja/resources/templates/)

# プロジェクト 06. 完全な Agent Harness を構築する（総仕上げ）

## やること

これは集大成となるプロジェクトです。最初の 5 つのプロジェクトで学んだことをすべて組み合わせ、フルベンチマークを実行し、そのあとクリーンアップを行って、品質を保ちやすい状態になっているかを確認します。

文書の取り込み、インデックス作成、引用ベースの Q&A、実行時の可観測性、そして読みやすく再開しやすい repo 状態までを含む、固定の多機能タスクセットを使います。まず弱い harness のベースラインで実行し、次に最も強い harness で実行し、そのあとクリーンアップして再実行します。最後に harness のアブレーション実験を行い、一度に 1 つずつ要素を取り除いて、実際に何が効いているのかを確かめます。

## ツール

- Claude Code or Codex
- Git
- Node.js + Electron
- 品質ドキュメントテンプレート
- 評価用ルーブリック
- 最初の 5 つのプロジェクトで蓄積したすべての harness コンポーネント

## Harness の仕組み

完全な harness: すべての仕組み + 可観測性 + アブレーション調査
