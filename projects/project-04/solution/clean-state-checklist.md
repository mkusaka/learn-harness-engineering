# クリーン状態チェックリスト

このチェックリストは、コミット前と各セッションの最後に実行してください。

## ビルド

- [ ] `npm run check` が型エラーなしで通る
- [ ] `npm run build` が正常に完了する

## アーキテクチャ

- [ ] `bash scripts/check-architecture.sh` が違反なしで通る
- [ ] renderer コードに `fs` または `path` の import がない
- [ ] service コードに Electron IPC がない
- [ ] services または main process に React の import がない

## 実行時

- [ ] アプリケーションがエラーなく起動する（`npm run dev`）
- [ ] 起動時に構造化ログがコンソールに出力される
- [ ] ドキュメントの import が動作する（`IMPORT_DOCUMENT` イベントのログを確認）
- [ ] あらゆるサイズのドキュメントで indexing が動作する
- [ ] Q&A が引用付きの回答を返す（`ASK_QUESTION` イベントのログを確認）

## データ整合性

- [ ] インデックス済みドキュメントに空の chunk がない（`GET_CHUNKS` で確認）
- [ ] Q&A 履歴が再起動後も保持される
- [ ] ドキュメントの metadata が実ファイルと一致している

## リポジトリ

- [ ] `git status` に意図しないファイルがない
- [ ] 機密データ（`.env`、credentials）がステージされていない
- [ ] `claude-progress.md` が現在の状態に更新されている
- [ ] `feature_list.json` が実際の機能状態を反映している
