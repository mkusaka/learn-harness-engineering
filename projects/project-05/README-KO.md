[英語版](./README.md) · **日本語**

# プロジェクト 05: 評価ループと 3役割のアップグレード

役割分離（単一役割 / 生成器+評価器 / 計画者+生成器+評価器）が、実装品質をどう変えるかを測定します。

エージェントシステムでは、役割分離は重要な品質向上の手法です。1つのエージェントが作業を行いながら自分の成果を評価すると、利益相反が起きます。別の評価器役割を導入すれば、より客観的な品質検証が可能になります。このプロジェクトでは、3つの構成を直接比較します。

## ディレクトリの説明

| ディレクトリ | 意味 |
|----------|------|
| `starter/` | **出発点** — P4 の solution を基にしており、マルチターンのQ&A履歴機能は実装待ちです。 |
| `solution/single-role/` | **変種 A** — 1つのエージェントがすべての作業（計画 + 実装 + 自己レビュー）を担当します。基本品質です。 |
| `solution/gen-eval/` | **変種 B** — generator + evaluator パターンです。修正の証跡があり、より高品質です。 |
| `solution/plan-gen-eval/` | **変種 C** — planner + generator + evaluator です。sprint contract と rubric を備えた最上位品質です。 |

## 使い方

```sh
# 3つの変種はそれぞれ独立して実行
cd solution/single-role && npm install  # 単一役割モード
cd solution/gen-eval && npm install     # 生成+評価モード
cd solution/plan-gen-eval && npm install # 完全な 3役割モード

# 3つの変種で次を比較します:
# - コード品質 (`evaluator-rubric.md` のスコア)
# - 見つかった欠陥数
# - 必要な手戻りの程度
```

## このプロジェクトで扱う機能

- マルチターンQ&A履歴（対話型 UI）
- sprint contract
- evaluator rubric の調整

## 関連講義

- [講義 09: エージェントが早すぎる完了宣言をしないようにする方法](../../docs/lectures/lecture-09-why-agents-declare-victory-too-early/index.md)
- [講義 10: パイプライン全体の実行だけが本当の検証である理由](../../docs/lectures/lecture-10-why-end-to-end-testing-changes-results/index.md)
