# Project 02: Agent-Readable Workspace

リポジトリの可読性と、明示的な継続用アーティファクトが、セッションをまたぐ開発でのコンテキスト喪失をどう減らすかを示します。

## ディレクトリ説明

| ディレクトリ | 意味 |
|------|------|
| `starter/` | **出発点**——P1 solution をベースにしたコードで、文書のインポート、詳細ビュー、永続化機能はまだ実装されていません。harness は弱めで、AGENTS.md は簡素、session-handoff もありません。 |
| `solution/` | **参考実装**——すべての新機能が実装済みで、完全な workspace ドキュメント（ARCHITECTURE.md、PRODUCT.md、session-handoff.md）が揃っています。 |

## 使い方

```sh
# 完了には少なくとも 2 つの agent セッションが必要
cd starter
npm install
# Session A: 文書インポートと詳細ビューを実装
# Session B: 永続化を実装（agent がコンテキストを素早く復元できるか確認）

cd ../solution
npm install
# 完全な harness で再実行し、セッション復元の速さを比較
```

## このプロジェクトで扱う機能

- 文書インポートの流れ（ファイル選択器 + IPC 送信）
- 文書詳細ビュー（メタデータ + 内容表示）
- 基本的な永続化（インポートした文書が再起動後も保持される）

## 対応する講義

- [Lecture 03: リポジトリを唯一の真実の情報源にする](../../docs/lectures/lecture-03-why-the-repository-must-become-the-system-of-record/index.md)
- [Lecture 04: 巨大な1つの指示ファイルではなく、指示ファイルを分割する](../../docs/lectures/lecture-04-why-one-giant-instruction-file-fails/index.md)
