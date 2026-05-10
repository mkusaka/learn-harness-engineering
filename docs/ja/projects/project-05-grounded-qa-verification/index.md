[英語版 →](../../../en/projects/project-05-grounded-qa-verification/) | [中国語版 →](../../../zh/projects/project-05-grounded-qa-verification/)

> 関連講義: [講義 09. エージェントが勝利宣言を早まるのを防ぐ](./../../lectures/lecture-09-why-agents-declare-victory-too-early/index.md) · [講義 10. フルパイプラインの実行だけが本当の検証になる](./../../lectures/lecture-10-why-end-to-end-testing-changes-results/index.md)
> テンプレートファイル: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/ja/resources/templates/)

# プロジェクト 05. エージェント自身に作業を検証させる

## やること

役割分担を実装します。実装を担う `generator`、レビューを行う `evaluator`、必要に応じて `planner` を分けます。3回実行して、役割を追加するたびの効果を測定してください。

多段会話、引用パネルの再設計、ドキュメントの絞り込みなど、実質的な機能改善を1つ選び、すべての実行で一貫して使ってください。

## ツール

- Claude Code or Codex
- Git
- Node.js + Electron

## ハーネスの仕組み

自己検証 + grounded Q&A + 証拠に基づく完了判定
