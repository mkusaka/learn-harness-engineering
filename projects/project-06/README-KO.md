[英語版](./README.md) · **日本語**

# プロジェクト 06: ランタイムの可観測性とデバッグ（キャップストーン）

講義の卒業プロジェクトです。完全なハーネス(harness)を構築し、ベンチマーク(benchmark)テストを実行し、クリーンアップ(cleanup)ループを回して、品質を維持できるかどうかを検証します。

このプロジェクトは、講義全体を総括するキャップストーン(capstone)です。これまでの5つのプロジェクトで学んだすべてのハーネスメカニズムを、ひとつの完成されたシステムに統合します。完成したハーネスは、将来のエージェントセッションが安定して作業を引き継げる土台を提供する必要があります。

## ディレクトリの説明

| ディレクトリ | 意味 |
|----------|------|
| `starter/` | **開始点** — 製品コードは完成していますが、ハーネスは意図的に弱められています（基本の AGENTS.md しかなく、feature_list.json、session-handoff、clean-state-checklist がありません）。 |
| `solution/` | **参考実装** — 最大構成のハーネスです。すべての成果物ファイルがそろっており、quality-document.md の評価が高く、ベンチマークスクリプトとクリーンアップスキャナーが含まれています。 |

## 使い方

```sh
cd starter
npm install
# 弱いハーネスでベンチマークスイートを実行し、結果を記録する

cd ../solution
npm install
# 完全なハーネスで同じベンチマークを実行する
# クリーンアップループを実行する
# quality-document.md のスコアの変化を比較する

# ベンチマークを実行
./scripts/benchmark.sh

# クリーンアップスキャンを実行
./scripts/cleanup-scanner.sh
```

## このプロジェクトで扱う機能

- ドキュメントの取得
- インデックスの構築または更新
- 引用付きの質疑応答
- ランタイムフィードバック(feedback)
- 読み取り可能で再開可能なリポジトリ状態

## 関連講義

- [講義 11: エージェントのランタイムを可観測にする方法](../../docs/lectures/lecture-11-why-observability-belongs-inside-the-harness/index.md)
- [講義 12: なぜすべてのセッションがクリーンな状態を残す必要があるのか](../../docs/lectures/lecture-12-why-every-session-must-leave-a-clean-state/index.md)
