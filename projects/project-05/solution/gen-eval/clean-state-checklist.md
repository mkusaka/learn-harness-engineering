# クリーン状態チェックリスト

このチェックリストは、コミット前と各セッションの終わりに実行してください。

## Build

- [ ] `npm run check` が型エラーなしで通る
- [ ] `npm run build` が正常に完了する

## Architecture

- [ ] `bash scripts/check-architecture.sh` が違反なしで通る
- [ ] renderer コードで `fs` または `path` を import していない
- [ ] service コードで Electron IPC を使っていない
- [ ] services または main process で React を import していない

## Runtime

- [ ] アプリケーションがエラーなく起動する（`npm run dev`）
- [ ] 起動時に構造化ログがコンソールに表示される
- [ ] ドキュメントの import が動作する（`IMPORT_DOCUMENT` イベントをログで確認）
- [ ] あらゆるサイズのドキュメントで indexing が動作する
- [ ] Q&A が出典付きの回答を返す（`ASK_QUESTION` イベントをログで確認）

## Data Integrity

- [ ] index 済みドキュメントに空の chunk がない（`GET_CHUNKS` で確認）
- [ ] Q&A 履歴が再起動後も保持される
- [ ] ドキュメントの metadata が実ファイルと一致している

## Repository

- [ ] `git status` に意図しないファイルがない
- [ ] 機密データ（`.env`、credentials）がステージされていない
- [ ] `claude-progress.md` が現在の状態に更新されている
- [ ] `feature_list.json` が実際の機能ステータスを反映している
