# Project 04: Runtime Observability and Structural Control

実行時の可観測性と構造化された境界チェックを導入しつつ、埋め込まれた実行時の不具合をデバッグします。

## 目录说明

| 目录 | 含义 |
|------|------|
| `starter/` | **出発点**。P3 solution をベースにしており、ログ出力と構造化された境界機能は未実装です。`IndexingService` には隠れた bug が埋め込まれていて、1000 文字を超えるファイルでは空のチャンクが生成されます。アーキテクチャ検査スクリプトはありません。 |
| `solution/` | **参考実装**。構造化ログモジュール、アーキテクチャ境界検査スクリプトがあり、埋め込まれていた bug は修正済みです。 |

## 使用方法

```sh
cd starter
npm install
# 1. エージェントがログから bug を特定できるか確認する
# 2. 大きなファイルを取り込み、チャンク分割の結果が異常でないか確認する

cd ../solution
npm install
# 比較: 構造化ログが問題診断をどれだけ速めるかを見る
```

## このプロジェクトで扱う機能

- 起動ログ
- 取り込みとインデックス作成のログ
- 目に見える問答失敗パス
- `main` / `preload` / `renderer` / `services` 層の明示的な境界
- 埋め込まれた実行時の不具合のデバッグ

## 対応する講義資料

- [Lecture 07: エージェントに明確な作業境界を与える](../../docs/lectures/lecture-07-why-agents-overreach-and-under-finish/index.md)
- [Lecture 08: 機能一覧でエージェントの振る舞いを制約する](../../docs/lectures/lecture-08-why-feature-lists-are-harness-primitives/index.md)
