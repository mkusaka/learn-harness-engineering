# Architecture -- Knowledge Base Electron App

## システム概要

Knowledge Base は、TypeScript と React で構築された Electron デスクトップアプリケーションです。ファイルピッカーを使ったドキュメントの取り込み、チャンク分割を伴うテキスト索引化、コンテンツ表示、出典付きの根拠ベース質問応答を提供します。

## レイヤー図

```
+-----------------------------------------------------------+
|                     Renderer (React)                       |
|  App.tsx -> DocumentList, DocumentDetail, ImportPanel,    |
|             QuestionPanel, StatusBar                       |
+-----------------------------------------------------------+
         |  window.knowledgeBase.* (型付き IPC ブリッジ)
+-----------------------------------------------------------+
|                     Preload スクリプト                    |
|  contextBridge.exposeInMainWorld -> documents, indexing, qa|
+-----------------------------------------------------------+
         |  ipcRenderer.invoke(IPC_CHANNELS.*)
+-----------------------------------------------------------+
|                     Main Process                           |
|  main.ts -> createWindow(), initializeServices()          |
|  ipc-handlers.ts -> registerIpcHandlers()                  |
+-----------------------------------------------------------+
         |  Service メソッド呼び出し
+-----------------------------------------------------------+
|                     Services レイヤー                    |
|  DocumentService | IndexingService | QaService             |
|  PersistenceService (ファイルシステム I/O)                 |
+-----------------------------------------------------------+
```

## Electron の各レイヤー

### Main Process (`src/main/`)

- **ウィンドウ管理**: セキュアな Web 設定を持つ `BrowserWindow` インスタンスを作成します。
- **IPC 登録**: `registerIpcHandlers()` を通じて、IPC チャンネル名を service メソッドに対応付けます。
- **Service 初期化**: 依存性注入を使ってすべての service を構築します。

### Preload (`src/preload/`)

preload スクリプトは、`contextBridge` 経由で型付き API を公開します。

```typescript
window.knowledgeBase = {
  documents: { list, import, get, getContent, delete },
  indexing:   { start, status, chunks },
  qa:         { ask, history },
}
```

### Renderer (`src/renderer/`)

Vite でバンドルされる React 18 アプリケーションです。

- `App.tsx` -- 取り込み切り替え、ドキュメント選択、Q&A を含むルートレイアウト。
- `DocumentList` -- 取り込んだドキュメントを一覧表示するサイドバー。
- `DocumentDetail` -- メタデータ、全文、チャンク、削除ボタンを表示します。
- `ImportPanel` -- `.txt` と `.md` ドキュメントを取り込むためのファイル入力。
- `QuestionPanel` -- 質問を入力するためのテキスト入力欄。
- `StatusBar` -- インデックス状態とドキュメント数を表示します。

### Services (`src/services/`)

- `PersistenceService` -- 原子的書き込みを伴う低レベルの JSON/text ファイル I/O。
- `DocumentService` -- コンテンツ保存とクリーンアップを含むドキュメント CRUD。
- `IndexingService` -- 段落を考慮したチャンク分割（約500文字）とインデックス管理。
- `QaService` -- キーワードベースの検索と引用を使ったモック Q&A。

## 取り込みフロー

ドキュメント取り込みフローは、IPC のデータパス全体を示しています。

```
1. ユーザーが App.tsx の "Import" ボタンをクリックする
2. ImportPanel がファイル入力を描画する
3. ユーザーが .txt または .md ファイルを選択する
4. ImportPanel が onImport(file.path) を呼び出す
5. App.tsx が window.knowledgeBase.documents.import(filePath) を呼び出す
6. Preload ブリッジが ipcRenderer.invoke('documents:import', filePath) を実行する
7. ipc-handlers.ts が DocumentService.importDocument(filePath) に処理を委譲する
8. DocumentService:
   a. ファイルが存在することを検証する
   b. ファイルの内容と統計情報を読み取る
   c. Document のメタデータオブジェクトを作成する
   d. PersistenceService 経由でファイルを documents ディレクトリにコピーする
   e. 抽出したテキストコンテンツを PersistenceService 経由で保存する
   f. documents-meta.json に追記する
9. 結果が IPC を通じて戻る
10. App.tsx が refreshDocuments() を呼び出して一覧を更新する
11. DocumentList が新しいドキュメントで再描画される
```

## コンテンツ取得フロー

ドキュメントコンテンツの表示には、専用の IPC チャンネルが追加されています。

```
1. ユーザーが DocumentDetail の "View Content" をクリックする
2. DocumentDetail が window.knowledgeBase.documents.getContent(id) を呼び出す
3. Preload が 'documents:get-content' IPC を呼び出す
4. ipc-handlers が DocumentService.getDocumentContent(id) に委譲する
5. PersistenceService が content/<id>.txt を読み取る
6. コンテンツが renderer に戻り、pre-wrap コンテナで表示される
```

## データ保存

```
knowledge-base-data/
  documents-meta.json     # Document メタデータ配列
  content/
    <doc-id>.txt          # ドキュメントごとの抽出済みテキストコンテンツ
  chunks/
    <doc-id>.json         # ドキュメントごとのチャンク配列
  index/
    index-meta.json       # Document ID から chunk ID への対応表
  qa-history.json         # Q&A の操作ログ
```
