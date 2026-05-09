[中国語版 →](../../../zh/projects/project-03-multi-session-continuity/)

> 関連講義: [Lecture 05. Keep context alive across sessions](./../../lectures/lecture-05-why-long-running-tasks-lose-continuity/index.md) · [Lecture 06. Initialize before every agent session](./../../lectures/lecture-06-why-initialization-needs-its-own-phase/index.md)
> テンプレートファイル: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/en/resources/templates/)

# Project 03. セッション再起動をまたいでエージェントを継続稼働させる

## 実施内容

エージェントにスコープ制御と検証ゲートを追加します。ドキュメントのチャンク分割、メタデータ抽出、インデックス作成の進捗表示、引用ベースの Q&A フローを実装してください。`feature_list.json` を使って機能の状態を管理し、1つずつ進めます。検証の証拠がないまま "pass" にしないでください。

2回実行します。1回目は制約を課さず、2回目は厳格に適用します。

## ツール

- Claude Code or Codex
- Git
- Node.js + Electron

## ハーネスの仕組み

進捗ログ + セッション引き継ぎ + マルチセッション継続
