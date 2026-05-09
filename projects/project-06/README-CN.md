# Project 06: Runtime Observability and Debugging (Capstone)

コースの集大成プロジェクトです。完全な harness を構築してベンチマークを実行し、クリーンアップ・ループを回して品質の保守性を検証します。

## ディレクトリの説明

| ディレクトリ | 内容 |
|------|------|
| `starter/` | **出発点** - 完全な製品コードがある一方で、harness は意図的に弱められています（基本的な `AGENTS.md` のみで、`feature_list.json`、`session-handoff`、`clean-state-checklist` はありません）。 |
| `solution/` | **参考実装** - 最大構成の harness です。成果物ファイルがすべてそろっており、品質ドキュメントの評価も高く、ベンチマークスクリプトとクリーンスキャナを含みます。 |

## 使い方

```sh
cd starter
npm install
# 弱い harness でベンチマークテストスイートを実行し、結果を記録する

cd ../solution
npm install
# 完全な harness で同じベンチマークを実行する
# クリーンアップ・ループを実行する
# `quality-document.md` のスコア変化を比較する

# ベンチマークを実行する
./scripts/benchmark.sh

# クリーンスキャンを実行する
./scripts/cleanup-scanner.sh
```

## このプロジェクトで扱う機能

- ドキュメントの取り込み
- インデックスの構築または更新
- 引用付きの Q&A
- 実行時フィードバック
- 読みやすく、再開可能なリポジトリ状態

## 対応する講義

- [Lecture 11: 代理のランタイムを可観測にする](../../docs/lectures/lecture-11-why-observability-belongs-inside-the-harness/index.md)
- [Lecture 12: すべてのセッションでクリーンな状態を残す](../../docs/lectures/lecture-12-why-every-session-must-leave-a-clean-state/index.md)
