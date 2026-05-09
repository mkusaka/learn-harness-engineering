[英語版 →](../../../en/projects/project-03-multi-session-continuity/) | [中国語版 →](../../../zh/projects/project-03-multi-session-continuity/)

> 関連講義: [講義 05. セッションをまたいで文脈を維持する](./../../lectures/lecture-05-why-long-running-tasks-lose-continuity/index.md) · [講義 06. 各 agent セッション前の初期化](./../../lectures/lecture-06-why-initialization-needs-its-own-phase/index.md)
> テンプレート: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/vi/resources/templates/)

# プロジェクト 03. セッション再起動をまたいで Agent に作業を継続させる

## あなたが行うこと

agent に範囲制御と検証ゲートを追加します。ドキュメントのチャンク分割、メタデータ抽出、インデックス作成の進捗表示、引用ベースの Q&A フローを実装してください。`feature_list.json` を使って機能の状態を追跡します。1 回につき 1 機能だけを扱い、検証の証拠がないまま "pass" にしないでください。

実行は 2 回行います。1 回目は制約なし、2 回目は厳格な実行で行います。

## ツール

- Claude Code または Codex
- Git
- Node.js + Electron

## Harness の仕組み

進捗ログ + セッション引き継ぎ + マルチセッション継続性
