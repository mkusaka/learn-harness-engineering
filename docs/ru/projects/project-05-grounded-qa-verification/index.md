[中国語版 →](../../../zh/projects/project-05-grounded-qa-verification/)

> 関連講義: [Lecture 09. エージェントに勝利宣言を急がせない](./../../lectures/lecture-09-why-agents-declare-victory-too-early/index.md) · [Lecture 10. 本当の検証といえるのは、パイプライン全体を最後まで実行したときだけ](./../../lectures/lecture-10-why-end-to-end-testing-changes-results/index.md)
> テンプレート: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/en/resources/templates/)

# Project 05. エージェントに自分の作業を自分で検証させる

## やること

コードを書く generator、レビューする evaluator、必要に応じて planner を分離して実装します。3回実行して、追加した各ロールの効果を測定してください。

多段階の対話、引用パネルの再設計、文書フィルタリングのいずれかを含む、意味のある機能アップグレードを選び、すべての実行で同じ内容を維持してください。

## ツール

- Claude Code または Codex
- Git
- Node.js + Electron

## harness の仕組み

自己検証 + 根拠付き Q&A + 証拠ベースの終了判定
