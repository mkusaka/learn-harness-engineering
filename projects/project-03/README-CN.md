# Project 03: スコープ制御と根拠に基づく検証

明示的なスコープ制御と検証ゲートが、成果物の正確性を高められるかを評価します。

## ディレクトリの説明

| ディレクトリ | 意味 |
|------|------|
| `starter/` | **出発点**——P2 solution をベースに、文書分割、メタデータ抽出、索引状態、基本的な Q&A 機能の追加実装が必要です。「一度に1機能」の方針制約はありません。 |
| `solution/` | **参考実装**——すべての機能が実装済みで、AGENTS.md には「一度に1機能」の方針が含まれています。`feature_list.json` には fail→pass の変化と検証証跡が示されています。 |

## 使い方

```sh
cd starter
npm install
# agent が複数の機能を同時に実装しようとするかを観察します（スコープ逸脱）

cd ../solution
npm install
# scope control で再実行し、機能提供の正確性を比較します
```

## このプロジェクトで扱う機能

- 文書分割（段落を考慮し、約 500 文字）
- メタデータ抽出（単語数、行数、段落数）
- インデックス状態を UI に表示
- 基本的な Q&A フロー、出典の引用付き

## 対応する講義

- [Lecture 05: セッションをまたいでコンテキストを維持する](../../docs/lectures/lecture-05-why-long-running-tasks-lose-continuity/index.md)
- [Lecture 06: 各セッションの前に初期化を行う](../../docs/lectures/lecture-06-why-initialization-needs-its-own-phase/index.md)
