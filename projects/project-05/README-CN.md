# Project 05: Evaluator Loops and Three-Role Upgrades

役割分離（単一役割 / 生成 + 評価 / 計画 + 生成 + 評価）が実装品質にどう影響するかを測定します。

## 目录说明

| 目录 | 含义 |
|------|------|
| `starter/` | **出発点** - P4 の solution をベースに、多輪の Q&A 履歴機能を新たに実装する。 |
| `solution/single-role/` | **変種 A** - 1 つのエージェントがすべての作業を担当する（計画 + 実装 + 自己レビュー）。基本品質。 |
| `solution/gen-eval/` | **変種 B** - 生成器 + 評価器のパターン。修正の証跡があり、より高い品質。 |
| `solution/plan-gen-eval/` | **変種 C** - 計画者 + 生成器 + 評価器。最も高品質で、スプリント契約と評価基準がある。 |

## 使用方法

```sh
# 3 つの変種はそれぞれ独立して実行する
cd solution/single-role && npm install  # 単一役割モード
cd solution/gen-eval && npm install     # 生成 + 評価モード
cd solution/plan-gen-eval && npm install # 完全な三役割モード

# 3 つの変種を比較する：
# - コード品質（evaluator-rubric.md の採点）
# - 見つかった欠陥の数
# - 手戻りの必要度
```

## このプロジェクトで扱う機能

- 多輪 Q&A 履歴（対話型 UI）
- スプリント契約（sprint contract）
- 評価器の採点基準（evaluator rubric）の調整

## 対応する講義

- [Lecture 09: エージェントが勝利を早まって宣言するのを防ぐ](../../docs/lectures/lecture-09-why-agents-declare-victory-too-early/index.md)
- [Lecture 10: 本当の検証は全工程を通して実行してこそ成立する](../../docs/lectures/lecture-10-why-end-to-end-testing-changes-results/index.md)
