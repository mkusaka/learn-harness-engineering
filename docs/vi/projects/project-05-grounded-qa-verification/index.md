[英語版 →](../../../en/projects/project-05-grounded-qa-verification/) | [中国語版 →](../../../zh/projects/project-05-grounded-qa-verification/)

> 関連講義: [第09講. エージェントが完了を早々に宣言するのを防ぐ](./../../lectures/lecture-09-why-agents-declare-victory-too-early/index.md) · [第10講. 真の検証は end-to-end testing だけである](./../../lectures/lecture-10-why-end-to-end-testing-changes-results/index.md)
> テンプレート: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/vi/resources/templates/)

# プロジェクト 05. エージェントに自分の作業を検証させる

## やること

役割分担を実装します。つまり、実行する generator、レビューする evaluator、そして任意で planner を用意します。3 回実行し、役割を追加するたびの影響を測定します。

実質的な機能改善を 1 つ選びます。たとえば、複数ターンの会話、citation panel の再設計、または文書フィルタリングです。そして、それをすべての実行で一貫して適用します。

## ツール

- Claude Code または Codex
- Git
- Node.js + Electron

## Harness の仕組み

自己検証 + grounding ありの Q&A + 証拠に基づく完了
