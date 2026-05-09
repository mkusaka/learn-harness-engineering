[中国語版 →](../../../zh/projects/project-06-runtime-observability-and-debugging/)

> 関連講義: [講義 11. runtime エージェントを観測可能にする](./../../lectures/lecture-11-why-observability-belongs-inside-the-harness/index.md) · [講義 12. 各セッションの最後にクリーンな handoff を行う](./../../lectures/lecture-12-why-every-session-must-leave-a-clean-state/index.md)
> テンプレートファイル: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/en/resources/templates/)

# プロジェクト 06. 完全なエージェント harness を組み立てる（Capstone）

## やること

これは capstone プロジェクトです。最初の 5 つのプロジェクトで学んだことをすべて組み合わせ、完全なベンチマークを実行したうえで、品質が保守可能であることを確認するためのクリーンアップを行います。

固定された multi-feature タスクのセットを使い、ドキュメントのインポート、インデックス作成、引用付き Q&A、runtime の観測可能性、そして読みやすく再起動可能なリポジトリ状態を含む、製品全体をカバーする範囲を扱います。まず弱い harness-baseline で実行し、次に最も強力な harness で実行し、その後でクリーンアップして再実行します。最後に harness の ablation 実験を行い、コンポーネントを 1 つずつ取り除きながら、何が本当に重要かを確認します。

## ツール

- Claude Code または Codex
- Git
- Node.js + Electron
- quality ドキュメントのテンプレート
- evaluator ルーブリック
- これまでの 5 つのプロジェクトで蓄積した harness のすべてのコンポーネント

## harness の仕組み

完全な harness: すべての仕組み + 観測可能性 + ablation 研究
