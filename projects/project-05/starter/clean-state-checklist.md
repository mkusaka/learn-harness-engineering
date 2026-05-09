# クリーン状態チェックリスト

コミット前と各セッションの最後に、このチェックリストを実行してください。

## ビルド

- [ ] `npm run check` が型エラーなしで通る
- [ ] `npm run build` が正常に完了する

## アーキテクチャ

- [ ] `bash scripts/check-architecture.sh` が違反なしで通る
- [ ] renderer コードに `fs` または `path` の import がない
- [ ] service コードに Electron IPC がない
- [ ] services または main process に React の import がない

## 実行時

- [ ] アプリケーションがエラーなく起動する (`npm run dev`)
- [ ] 起動時に構造化ログがコンソールへ出力される
- [ ] ドキュメントのインポートが動作する（ログで `IMPORT_DOCUMENT` イベントを確認する）
- [ ] あらゆるサイズのドキュメントでインデックス作成が動作する
- [ ] Q&A が引用付きで回答を返す（ログで `ASK_QUESTION` イベントを確認する）

## データ整合性

- [ ] インデックス済みドキュメントに空のチャンクがない（`GET_CHUNKS` で確認する）
- [ ] Q&A の履歴が再起動後も保持される
- [ ] ドキュメントのメタデータが実際のファイルと一致している

## リポジトリ

- [ ] `git status` に意図しないファイルがない
- [ ] 機密データ（`.env`、認証情報）が stage されていない
- [ ] `claude-progress.md` が現在の状態に更新されている
- [ ] `feature_list.json` が実際の機能状況を反映している
