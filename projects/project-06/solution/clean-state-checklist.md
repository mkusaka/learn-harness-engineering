# クリーン状態チェックリスト

このチェックリストは、コミット前と各セッションの最後に実行してください。

## ビルド

- [ ] `npm run check` が型エラーなしで通る
- [ ] `npm run build` が正常に完了する
- [ ] 未使用の変数や import に関する TypeScript 警告がない

## アーキテクチャ

- [ ] レンダラーコード (`src/renderer/`) に `fs` または `path` の import がない
- [ ] サービスコード (`src/services/`) に Electron IPC がない
- [ ] services や main process に React の import がない
- [ ] すべての IPC チャネルが `src/shared/types.ts` に定義されている
- [ ] 新しい API はすべて `src/preload/preload.ts` で公開されている

## 実行時

- [ ] アプリケーションがエラーなく起動する (`npm run dev`)
- [ ] 起動時に構造化された JSON ログがコンソールに出力される
- [ ] ドキュメントのインポートが動作する (`"Document imported"` イベントをログで確認)
- [ ] バッチインデックス作成が動作する (`"Batch indexing complete"` イベントをログで確認)
- [ ] Q&A が引用付きの回答を返す (`"Answer generated"` イベントをログで確認)
- [ ] 会話履歴がチャット風のレイアウトで表示される
- [ ] Q&A 応答にフィードバックボタンが表示される
- [ ] リセットボタンが確認ダイアログ付きで全データを消去する
- [ ] ステータスバーに正しいドキュメント数とインデックス状態が表示される

## ロギング

- [ ] すべてのログエントリが有効な JSON である（解析可能）
- [ ] ログエントリに timestamp, level, service, message が含まれている
- [ ] ドキュメントのインポートで documentId, filename, size を含む INFO ログが出る
- [ ] インデックス作成で chunkCount, durationMs を含む INFO ログが出る
- [ ] Q&A で confidence, citationCount, durationMs を含む INFO ログが出る
- [ ] IPC ハンドラがチャネル呼び出しをログ出力する

## データ整合性

- [ ] インデックス済みドキュメントに空のチャンクがない（`GET_CHUNKS` で確認）
- [ ] Q&A 履歴が再起動後も保持される
- [ ] フィードバックの記録が再起動後も保持される
- [ ] ドキュメントのメタデータが実ファイルと一致している
- [ ] クリーン状態へのリセットで全データファイルが削除される

## パフォーマンス

- [ ] `bash scripts/benchmark.sh` がエラーなく実行できる
- [ ] インポートのスループットが妥当である（3 ファイルで 1 秒未満）
- [ ] サンプルデータのインデックス作成が 1 秒未満で完了する
- [ ] クエリのレイテンシが質問あたり 1 秒未満である

## リポジトリ

- [ ] `git status` に意図しないファイルがない
- [ ] 機密データ（`.env`、認証情報）がステージされていない
- [ ] `dist/` 配下のファイルがコミットされていない
- [ ] `claude-progress.md` が現在の状態で更新されている
- [ ] `feature_list.json` が実際の機能状態を反映している
- [ ] セッション終了時は `session-handoff.md` が更新されている

## スクリプト

- [ ] `bash scripts/cleanup-scanner.sh` で古い成果物が検出されない
- [ ] `bash scripts/benchmark.sh` が全タスクスイートを完了する
- [ ] `bash init.sh` がすべての検証ステップを通過する
