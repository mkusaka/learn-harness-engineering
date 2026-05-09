[英語版 →](../../../en/projects/project-06-runtime-observability-and-debugging/)

> 関連講義: [講義 11. エージェントのランタイムを可観測にする](./../../lectures/lecture-11-why-observability-belongs-inside-the-harness/index.md) · [講義 12. すべてのセッションの終わりでクリーンにハンドオフする](./../../lectures/lecture-12-why-every-session-must-leave-a-clean-state/index.md)
> テンプレートファイル: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/en/resources/templates/)

# プロジェクト 06. 完全なエージェントハーネスを構築する（キャップストーン）

## やること

このプロジェクトはキャップストーン(capstone)プロジェクトです。これまでの5つのプロジェクトで学んだ内容をすべて組み合わせ、全体のベンチマーク(benchmark)を実行し、その後で品質を維持できるかを確認するためのクリーンアップ処理(cleanup pass)を行います。

完全なプロダクトスライス(product slice)をカバーする、固定されたマルチ機能のタスクセットを使います。ドキュメントのインポート、インデックス作成、引用ベースのQ&A、ランタイムの可観測性、そして読みやすく再起動可能なリポジトリ状態が含まれます。まず弱いハーネスのベースラインで実行し、次に最も強力なハーネスで実行したうえで、クリーンアップ後に再実行します。最後にハーネスのアブレーション実験(harness ablation experiment)を行います。構成要素を1つずつ取り除きながら、実際に何が重要なのかを確認します。

前のプロジェクトで個別に効果を確認したメカニズムが一緒に動くときにどのような相乗効果が生まれるのか、そしてどの構成要素を取り除くと品質が最も大きく低下するのかを把握することが、このキャップストーンの核心です。

## ツール

- Claude Code または Codex
- Git
- Node.js + Electron
- 品質ドキュメントテンプレート(quality document template)
- 評価者ルーブリック(evaluator rubric)
- これまでの5つのプロジェクトで蓄積したすべてのハーネス構成要素

## ハーネスメカニズム

完全なハーネス: すべてのメカニズム + 可観測性 + アブレーション研究(ablation study)
