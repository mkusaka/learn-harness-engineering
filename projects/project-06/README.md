# Project 06: 実行時の可観測性とデバッグ（Capstone）

Capstone プロジェクトです。完全な harness を構築してベンチマークを実行し、その後 cleanup loop を回して品質と保守性を検証します。

## ディレクトリガイド

| Directory | Meaning |
|------|------|
| `starter/` | **開始地点**: 製品コードは完成していますが、harness は意図的に弱められています（基本的な AGENTS.md しかなく、feature_list.json、session handoff、clean-state checklist はありません）。 |
| `solution/` | **参考実装**: 最大構成の harness で、すべての artifact ファイル、質の高い quality-document スコア、benchmark スクリプト、cleanup scanner が揃っています。 |

## 使い方

```sh
cd starter
npm install
# 弱い harness で benchmark suite を実行し、結果を記録する

cd ../solution
npm install
# 完全な harness で同じ benchmark を実行する
# cleanup loop を実行する
# quality-document.md のスコア変化を比較する

# benchmark テストを実行する
./scripts/benchmark.sh

# cleanup scan を実行する
./scripts/cleanup-scanner.sh
```

## 対象機能

- Import documents
- Build or refresh the index
- Answer questions with citations
- Runtime feedback
- Readable, restartable repository state

## 関連講義

- [Lecture 11: Why Observability Belongs Inside the Harness](../../docs/en/lectures/lecture-11-why-observability-belongs-inside-the-harness/index.md)
- [Lecture 12: Why Every Session Must Leave a Clean State](../../docs/en/lectures/lecture-12-why-every-session-must-leave-a-clean-state/index.md)
