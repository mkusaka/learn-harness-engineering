[英語版 →](../../../en/projects/project-05-grounded-qa-verification/)

> 関連講義: [講義 09. エージェントが早まって完了を宣言しないようにする](./../../lectures/lecture-09-why-agents-declare-victory-too-early/index.md) · [講義 10. 完全なパイプライン実行だけが真の検証として認められる](./../../lectures/lecture-10-why-end-to-end-testing-changes-results/index.md)
> テンプレートファイル: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/en/resources/templates/)

# プロジェクト 05. エージェントに自分の作業を検証させる

## やること

役割分担(role separation)を実装します。実装を担当する generator、レビューを担当する evaluator、そして必要に応じて planner を作成します。各役割を追加するたびに、効果を測定するために3回実行します。

エージェントが実装と検証を同時に担うと、自分の誤りを自力で見つけにくくなります。独立した evaluator の役割を分離すれば、hallucination や早すぎる完了宣言を防ぎ、grounded Q&A の精度を高められます。

実質的な機能アップグレード（マルチターン対話、引用パネルの再設計、またはドキュメントフィルタリング）を選び、すべての実行で同じ内容を維持します。

## ツール

- Claude Code または Codex
- Git
- Node.js + Electron

## ハーネスメカニズム

自己検証(self-verification) + grounded Q&A + evidence-based completion
