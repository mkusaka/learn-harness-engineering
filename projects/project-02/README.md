# Project 02: エージェントが読みやすいワークスペース

リポジトリの読みやすさと、明示的な継続用アーティファクトが、複数セッションにまたがる開発でのコンテキスト喪失をどのように減らすかを示します。

## ディレクトリガイド

| Directory | Meaning |
|------|------|
| `starter/` | **開始地点**: P1 のソリューションをベースにしており、ドキュメントの取り込み、詳細ビュー、永続化はまだ実装されていません。ハーネスは弱く、AGENTS.md は最小限で、セッション引き継ぎもありません。 |
| `solution/` | **参照実装**: すべての新機能が実装済みで、ワークスペースのドキュメント (ARCHITECTURE.md, PRODUCT.md, session-handoff.md) も揃っています。 |

## 使い方

```sh
# 完了には少なくとも 2 回のエージェントセッションが必要です
cd starter
npm install
# セッション A: ドキュメントの取り込みと詳細ビューを実装する
# セッション B: 永続化を実装する（エージェントがすばやくコンテキストを取り戻せるか確認する）

cd ../solution
npm install
# 完成版のハーネスで再実行し、セッション復帰速度を比較する
```

## 対象機能

- ドキュメント取り込みフロー（ファイルピッカーと IPC 転送）
- ドキュメント詳細ビュー（メタデータと内容の表示）
- 基本的な永続化（取り込んだドキュメントが再起動後も残る）

## 関連講義

- [Lecture 03: Why the Repository Must Become the System of Record](../../docs/en/lectures/lecture-03-why-the-repository-must-become-the-system-of-record/index.md)
- [Lecture 04: Why One Giant Instruction File Fails](../../docs/en/lectures/lecture-04-why-one-giant-instruction-file-fails/index.md)
