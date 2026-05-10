# AGENTS.md

このリポジトリは、長時間にわたる coding-agent の作業に最適化されています。このファイルは
短く保ってください。巨大な指示の塊にするのではなく、system-of-record の文書へ誘導する
ルーティング層として使ってください。

## 起動ワークフロー

コードを変更する前に:

1. `pwd` でリポジトリのルートを確認する。
2. `ARCHITECTURE.md` を読み、現在のシステム構成図と厳格な依存ルールを把握する。
3. `docs/QUALITY_SCORE.md` を読み、どのドメインやレイヤーが最も弱いか確認する。
4. `docs/PLANS.md` を読み、その後で作業中のアクティブな plan を開く。
5. `docs/product-specs/` にある関連する product spec を読む。
6. このリポジトリで標準の bootstrap と verification の手順を実行する。
7. ベースラインの verification が失敗している場合は、スコープを追加する前にベースラインを修復する。

## ルーティングマップ

- `ARCHITECTURE.md`: ドメインマップ、レイヤーモデル、依存ルール
- `docs/design-docs/index.md`: 設計判断と基本方針
- `docs/product-specs/index.md`: 現在の製品挙動と受け入れ基準
- `docs/PLANS.md`: plan のライフサイクルと execution-plan ポリシー
- `docs/QUALITY_SCORE.md`: 製品ドメインとレイヤーの健全性
- `docs/RELIABILITY.md`: 実行時シグナル、ベンチマーク、再起動時の期待値
- `docs/SECURITY.md`: secret、sandbox、データ、外部アクションのルール
- `docs/FRONTEND.md`: UI 制約、デザインシステムのルール、アクセシビリティ確認

## 作業契約

- 1つの境界づけられた plan か feature slice を一度に1つずつ扱う。
- コードを読むだけで完了扱いにしないこと。実行可能な証拠が必要です。
- 挙動を変更した場合は、同じセッション内で対応する product、plan、または reliability の文書を更新する。
- 同じ指摘がレビューで繰り返されるなら、チャットで言い直すのではなく、機械的なルール、チェック、または linter に落とし込む。
- 生成物は `docs/generated/` に、ソース参照は `docs/references/` に置く。
- このファイルを大きくするより、小さくて新しい文書を追加することを優先する。

## 完了条件

変更が完了したとみなせるのは、次のすべてが真である場合だけです:

- 対象の挙動が実装されている
- 必要な verification が実際に実行された
- 証拠が関連する plan または quality document にリンクされている
- 影響を受けた文書が最新の状態に保たれている
- 標準の起動手順からリポジトリを問題なく再起動できる

## セッション終了時

セッションを終える前に:

1. アクティブな execution plan を更新する。
2. どのドメインまたはレイヤーでも意味のある変更があったなら `docs/QUALITY_SCORE.md` を更新する。
3. 保留にした技術的負債は `docs/exec-plans/tech-debt-tracker.md` に記録する。
4. 該当する場合は、完了した plan を `docs/exec-plans/completed/` に移す。
5. 次の明確なアクションが分かる、再起動可能な状態でリポジトリを残す。
