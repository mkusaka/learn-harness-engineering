# Project 03: スコープ制御と根拠に基づく検証

明示的なスコープ制御と検証ゲートが、成果物の精度を向上させるかを評価します。

## ディレクトリガイド

| Directory | Meaning |
|------|------|
| `starter/` | **開始地点**: P2 の解答をベースにしており、ドキュメントのチャンク分割、メタデータ抽出、インデックス状態、基本的な QA はまだ実装する必要があります。「1 度に 1 機能ずつ」という戦略制約はありません。 |
| `solution/` | **参照実装**: すべての機能が実装済みです。AGENTS.md には「1 度に 1 機能ずつ」の戦略が含まれており、feature_list.json には fail-to-pass の移行と検証の証跡が示されています。 |

## 使い方

```sh
cd starter
npm install
# エージェントが複数の機能を同時に実装しないかを観察します（スコープの逸脱）

cd ../solution
npm install
# スコープ制御ありで再実行し、機能の実装精度を比較します
```

## 対象機能

- ドキュメントのチャンク分割（段落を考慮し、約 500 文字）
- メタデータ抽出（単語数、行数、段落数）
- UI へのインデックス状態表示
- 出典を示す基本的な QA フロー

## 関連講義

- [Lecture 05: なぜ長時間タスクは継続性を失うのか](../../docs/en/lectures/lecture-05-why-long-running-tasks-lose-continuity/index.md)
- [Lecture 06: なぜ初期化には独自のフェーズが必要なのか](../../docs/en/lectures/lecture-06-why-initialization-needs-its-own-phase/index.md)
