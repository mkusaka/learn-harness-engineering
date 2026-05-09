[英語版 →](../../../en/projects/project-06-runtime-observability-and-debugging/) | [中国語版 →](../../../zh/projects/project-06-runtime-observability-and-debugging/)

> 関連講義: [第11回. エージェントの runtime を観測可能にする](./../../lectures/lecture-11-why-observability-belongs-inside-the-harness/index.md) · [第12回. 各セッションの終了時にクリーンな状態へ戻す](./../../lectures/lecture-12-why-every-session-must-leave-a-clean-state/index.md)
> テンプレート: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/vi/resources/templates/)

# プロジェクト 06. 完全な Harness Agent を構築する（Capstone）

## 取り組む内容

これは capstone プロジェクトです。最初の 5 つのプロジェクトで学んだことをすべて統合し、完全なベンチマークを実行したうえで、品質を継続的に維持できることを確認するために 1 回クリーンアップを行います。

ドキュメントのインポート、インデックス作成、引用ベースの Q&A、runtime の可観測性、そして読み取りと再起動が可能なリポジトリ状態を含む、固定の多機能タスクセットを使用します。まず弱いベースライン harness で 1 回実行し、次に最も強力な harness で再実行し、その後にクリーンアップしてもう一度実行します。最後に harness のアブレーション実験を行い、各コンポーネントを 1 つずつ取り除いて、何が本当に重要なのかを確認します。

## ツール

- Claude Code または Codex
- Git
- Node.js + Electron
- ドキュメント品質テンプレート
- Rubric evaluator
- 最初の 5 つのプロジェクトで積み上げたすべての harness コンポーネント

## Harness の仕組み

完全な harness: すべての仕組み + 可観測性 + アブレーション研究
