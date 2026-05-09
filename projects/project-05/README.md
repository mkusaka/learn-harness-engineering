# Project 05: Evaluator Loops と 3 役への拡張

役割分担（単一ロール、generator と evaluator、planner と generator と evaluator）によって実装品質がどう変わるかを測ります。

## ディレクトリ案内

| ディレクトリ | 意味 |
|------|------|
| `starter/` | **開始地点**: P4 のソリューションをベースにしており、マルチターン QA 履歴はまだ実装が必要です。 |
| `solution/single-role/` | **Variant A**: 1 つの agent がすべての作業（計画、実装、自己レビュー）を担当します。基準となる品質です。 |
| `solution/gen-eval/` | **Variant B**: generator と evaluator のパターンです。修正の証跡があり、より高い品質になります。 |
| `solution/plan-gen-eval/` | **Variant C**: planner と generator と evaluator の構成です。スプリント契約と採点基準があり、最も高い品質になります。 |

## 使い方

```sh
# 3 つの variant をそれぞれ独立して実行します
cd solution/single-role && npm install  # single-role mode
cd solution/gen-eval && npm install     # generator plus evaluator mode
cd solution/plan-gen-eval && npm install # full three-role mode

# 3 つの variant を比較します:
# - コード品質（evaluator-rubric.md のスコア）
# - 見つかった欠陥の数
# - 必要になった手戻りの量
```

## 対応機能

- マルチターン QA 履歴（会話型 UI）
- スプリント契約
- evaluator rubric の調整

## 関連講義

- [Lecture 09: Why Agents Declare Victory Too Early](../../docs/en/lectures/lecture-09-why-agents-declare-victory-too-early/index.md)
- [Lecture 10: Why End-to-End Testing Changes Results](../../docs/en/lectures/lecture-10-why-end-to-end-testing-changes-results/index.md)
