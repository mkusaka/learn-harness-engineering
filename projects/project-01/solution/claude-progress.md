# claude-progress.md -- セッションログ

## Project 01: ベースライン vs 最小ハーネス

### セッション 1 -- 2026-03-30

**Duration**: ~45 minutes
**Goal**: 適切なハーネスを備えたベースラインの Electron アプリを確立する

**What was done**:
- Electron ウィンドウが 1200x800 で起動し、正しい `webPreferences` が設定されていることを確認した
- ドキュメント一覧パネルが空状態メッセージを表示することを確認した
- 質問パネルが入力を受け付け、IPC 経由で送信できることを確認した
- `PersistenceService` が `userData` 配下にデータディレクトリを作成することを確認した
- `feature_list.json` を更新し、4 つすべての機能のステータスを `"pass"` にした
- 起動ルールとレイヤー境界を記した `AGENTS.md` を作成した
- Electron のレイヤー構成を説明する `docs/ARCHITECTURE.md` を作成した
- ナレッジベース要件を説明する `docs/PRODUCT.md` を作成した

**Decisions**:
- サービスをテストしやすく保つため、`PersistenceService` にはコンストラクタインジェクションを使った
- すべての IPC チャンネル名は `types.ts` の 1 つの `const` オブジェクトにまとめた
- 一貫性のため、ウィンドウタイトルは `"Knowledge Base"` に設定した

**Issues**: None

**Next session**: Project 02 に進み、import、detail view、persistence 機能を追加する。
