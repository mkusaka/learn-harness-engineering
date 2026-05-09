[中国語版 →](../../../zh/projects/project-03-multi-session-continuity/)

> 関連講義: [講義 05. セッション間でコンテキストを生かし続ける](./../../lectures/lecture-05-why-long-running-tasks-lose-continuity/index.md) · [講義 06. エージェントの各セッション前に初期化する](./../../lectures/lecture-06-why-initialization-needs-its-own-phase/index.md)
> テンプレートファイル: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/en/resources/templates/)

# プロジェクト 03. セッションの再起動をまたいでエージェントに作業を継続させる

## やること

エージェントにスコープ管理と検証用ゲートを追加してください。ドキュメントのチャンク分割、メタデータ抽出、インデックス作成の進捗表示、引用付きの Q&A フローを実装します。`feature_list.json` を使って機能の状態を追跡し、1 度に 1 機能だけを進めて、検証の証拠がないまま「pass」にはしないでください。

タスクは 2 回実行します。1 回目は制約なし、2 回目は厳格なコントロール付きです。

## ツール

- Claude Code または Codex
- Git
- Node.js + Electron

## harness の仕組み

進捗ログ + セッション間の handoff + セッションをまたいだ継続性
