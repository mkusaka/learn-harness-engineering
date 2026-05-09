# AGENTS.md

このリポジトリは、長時間稼働する coding-agent の作業に最適化されています。このファイルは短く保ってください。大きな指示書ではなく、記録されたシステム文書へのルーティング層として使ってください。

## 起動手順

コードを変更する前に:

1. `pwd` で repo のルートディレクトリを確認してください。
2. `ARCHITECTURE.md` を読んで、現在のシステムマップと厳格な依存ルールを把握してください。
3. `docs/QUALITY_SCORE.md` を読んで、どの domain または layer が最も弱いかを確認してください。
4. `docs/PLANS.md` を読んだうえで、そこから作業中の active plan を開いてください。
5. `docs/product-specs/` にある関連する製品仕様を読んでください。
6. この repo の bootstrap パスと標準の検証を実行してください。
7. baseline の検証が失敗している場合は、作業範囲を追加する前に baseline を修正してください。

## ルーティングマップ

- `ARCHITECTURE.md`: domain マップ、レイヤーモデル、依存ルール
- `docs/design-docs/index.md`: 設計上の意思決定と中核となる前提
- `docs/product-specs/index.md`: 現在の製品挙動と受け入れ目標
- `docs/PLANS.md`: plan のライフサイクルと実行ポリシー
- `docs/QUALITY_SCORE.md`: 製品 domain と layer の健全性
- `docs/RELIABILITY.md`: runtime シグナル、benchmark、再起動の期待値
- `docs/SECURITY.md`: secret、sandbox、データ、外部アクションのルール
- `docs/FRONTEND.md`: UI 制約、design system のルール、accessibility チェック

## 作業契約

- 境界が明確な plan か、機能の slice 1つずつを基点に作業してください。
- コードの確認だけで完了扱いにしないでください。実行可能な証拠が必要です。
- 振る舞いを変更した場合は、同じセッション内で該当する製品文書、plan、または reliability 文書を更新してください。
- 同じレビュー指摘が繰り返し出るなら、チャットで説明し直すのではなく、機械的なルール、テスト、または linter に落とし込んでください。
- 生成された文書は `docs/generated/` に、ソース参照文書は `docs/references/` に保ってください。
- このファイルを育てるより、小さくて最新の文書を追加することを優先してください。

## 完了定義

変更が完了とみなされるのは、次のすべてが満たされている場合だけです:

- 対象の挙動が実装されている
- 必要な検証が実際に実行されている
- 証拠が plan または関連する品質文書から参照できる
- 影響を受けた文書が最新の状態に保たれている
- repo を標準の起動パスからクリーンに再起動できる

## セッション終了時

セッションを終える前に:

1. active な実行 plan を更新してください。
2. どの domain または layer かが意味のある形で変わった場合は、`docs/QUALITY_SCORE.md` を更新してください。
3. 後回しにした新しい負債があるなら、`docs/exec-plans/tech-debt-tracker.md` に記録してください。
4. 完了した plan は、適切であれば `docs/exec-plans/completed/` に移してください。
5. 次に取るべき行動が明確で、repo を再起動可能な状態に保ってください。
