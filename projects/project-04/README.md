# Project 04: ランタイム可観測性と構造的制御

仕込まれたランタイム不具合をデバッグしながら、ランタイム可観測性と構造的な境界チェックを導入します。

## ディレクトリ案内

| ディレクトリ | 意味 |
|------|------|
| `starter/` | **開始地点**: P3 の解答をベースにしており、ログ出力と構造的な境界機能はまだ実装していません。`IndexingService` には隠された仕込みバグがあり、1000 文字を超えるファイルで空のチャンクが生成されます。アーキテクチャ確認スクリプトはありません。 |
| `solution/` | **参考実装**: 構造化ログモジュール、アーキテクチャ境界チェック用スクリプト、そして仕込みバグの修正済み版です。 |

## 使い方

```sh
cd starter
npm install
# 1. ログを通じてエージェントがバグを見つけられるか確認する
# 2. 大きなファイルを取り込み、チャンク分割が誤動作するか確認する

cd ../solution
npm install
# 構造化ログで原因特定がどれだけ速くなるか比較する
```

## 対応機能

- 起動ログ
- インポートとインデックス作成のログ
- 目に見える QA 失敗経路
- main、preload、renderer、services 各レイヤーの明確な境界
- 仕込み済みランタイム不具合のデバッグ

## 関連講義

- [Lecture 07: Why Agents Overreach and Under-Finish](../../docs/en/lectures/lecture-07-why-agents-overreach-and-under-finish/index.md)
- [Lecture 08: Why Feature Lists Are Harness Primitives](../../docs/en/lectures/lecture-08-why-feature-lists-are-harness-primitives/index.md)
