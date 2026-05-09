# セッション引き継ぎ -- Project 02

## 前回のセッション: 2026-03-30

### 完了したこと

1. **Document Import** -- 取り込みフローを一通り実装しました:
   - `.txt` と `.md` ファイルを選べる ImportPanel コンポーネント
   - App.tsx で取り込み画面の切り替えと、取り込み後の文書一覧の再読み込み
   - DocumentService.importDocument() によるファイルのコピーと内容の保存

2. **内容付きの Document Detail** -- DocumentDetail コンポーネントを拡張しました:
   - 文書テキストを取得する `getContent` IPC チャンネルを追加
   - "View Content" ボタンで全文を読み込み、表示
   - Delete ボタンでメタデータとファイルシステムの両方から文書を削除

3. **基本的な永続化** -- 文書が再起動後も保持されるようになりました:
   - App.tsx が useEffect 経由でマウント時に refreshDocuments() を呼び出す
   - PersistenceService が起動時に documents-meta.json を読み込む
   - すべての文書データを userData/knowledge-base-data/ 以下に保存

### 残っていること

Project 02 に残っている機能はありません。feature_list.json の 7 機能はすべて status "pass" です。

### 決定事項

- 一覧表示時のペイロードを小さく保つため、内容を GET_DOCUMENT にまとめず、GET_DOCUMENT_CONTENT を新しい IPC チャンネルとして追加しました。
- 文書削除では、保存済みの内容ファイルと documents ディレクトリ内の元のコピーの両方を削除します。
- 取り込みパネルはモーダルではなく、文書詳細ビューを置き換える形にしました。

### 変更したファイル

- `src/shared/types.ts` -- GET_DOCUMENT_CONTENT IPC チャンネルを追加
- `src/main/ipc-handlers.ts` -- GET_DOCUMENT_CONTENT ハンドラを登録
- `src/preload/preload.ts` -- documents.getContent() を公開
- `src/renderer/types.d.ts` -- getContent の型宣言を追加
- `src/renderer/App.tsx` -- 取り込み切り替え、削除ハンドラ、マウント時の再読み込みを追加
- `src/renderer/components/DocumentDetail.tsx` -- 内容ビューア、削除ボタン
- `src/services/document-service.ts` -- deleteDocument() を強化し、hasPersistedData() を追加
- `AGENTS.md` -- ドキュメント階層とセッション引き継ぎの案内を更新
- `feature_list.json` -- すべての機能を pass に変更
- `docs/ARCHITECTURE.md` -- 取り込みフローを更新
- `docs/PRODUCT.md` -- 新機能を更新

### ブロッカー

なし。

### 次のステップ

Project 03 に進み、インデックス作成、メタデータ抽出、根拠付き Q&A 機能を追加します。
