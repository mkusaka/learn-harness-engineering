# アーキテクチャ -- Knowledge Base Electron App

## システム概要

Knowledge Base は、TypeScript と React で構築された Electron デスクトップアプリケーションです。ファイルピッカーを使ったドキュメントの取り込み、チャンク分割を伴うテキスト索引化、内容表示、引用付きの根拠ある Q&A を提供します。

## レイヤー図

```
+-----------------------------------------------------------+
|                     Renderer (React)                       |
|  App.tsx -> DocumentList, DocumentDetail, ImportPanel,    |
|             QuestionPanel, StatusBar                       |
+-----------------------------------------------------------+
         |  window.knowledgeBase.* (型付き IPC ブリッジ)
+-----------------------------------------------------------+
|                     Preload Script                         |
|  contextBridge.exposeInMainWorld -> documents, indexing, qa|
+-----------------------------------------------------------+
         |  ipcRenderer.invoke(IPC_CHANNELS.*)
+-----------------------------------------------------------+
|                     メインプロセス                        |
|  main.ts -> createWindow(), initializeServices()          |
|  ipc-handlers.ts -> registerIpcHandlers()                  |
+-----------------------------------------------------------+
         |  サービスメソッド呼び出し
+-----------------------------------------------------------+
|                     サービス層                            |
|  DocumentService | IndexingService | QaService             |
|  PersistenceService (filesystem I/O)                       |
+-----------------------------------------------------------+
```

## Electron のレイヤー

### Main Process (`src/main/`)

- **ウィンドウ管理**: セキュアな web preferences を設定した `BrowserWindow` インスタンスを作成します。
- **IPC 登録**: `registerIpcHandlers()` を通じて IPC チャンネル名をサービスメソッドに割り当てます。
- **サービス初期化**: 依存性注入を使ってすべてのサービスを構築します。

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

Vite でバンドルされた React 18 アプリケーションです。

- `App.tsx` -- インポート切り替え、ドキュメント選択、Q&A を備えたルートレイアウト。
- `DocumentList` -- 取り込んだドキュメントのサイドバー一覧。
- `DocumentDetail` -- メタデータ、全文、チャンク、削除ボタンを表示します。
- `ImportPanel` -- `.txt` と `.md` ドキュメントを取り込むためのファイル入力。
- `QuestionPanel` -- 質問入力用のテキストボックス。
- `StatusBar` -- インデックスの状態とドキュメント数を表示します。

### Services (`src/services/`)

- `PersistenceService` -- アトミック書き込みを伴う低レベルの JSON/text ファイル I/O。
- `DocumentService` -- コンテンツ保存とクリーンアップを含むドキュメント CRUD。
- `IndexingService` -- 段落を考慮したチャンク分割（約 500 文字）とインデックス管理。
- `QaService` -- キーワードベースの検索と引用を使ったモック Q&A。

## インポートフロー

ドキュメントのインポートフローは、IPC の完全なデータ経路を示しています。

```
1. ユーザーが App.tsx の "Import" ボタンをクリックする
2. ImportPanel がファイル入力を表示する
3. ユーザーが .txt または .md ファイルを選択する
4. ImportPanel が onImport(file.path) を呼び出す
5. App.tsx が window.knowledgeBase.documents.import(filePath) を呼び出す
6. preload ブリッジが ipcRenderer.invoke('documents:import', filePath) を呼び出す
7. ipc-handlers.ts が DocumentService.importDocument(filePath) に処理を委譲する
8. DocumentService:
   a. ファイルの存在を検証する
   b. ファイルの内容と統計情報を読み取る
   c. Document のメタデータオブジェクトを作成する
   d. PersistenceService を通じてファイルを documents ディレクトリへコピーする
   e. 抽出したテキスト内容を PersistenceService を通じて保存する
   f. documents-meta.json に追記する
9. 結果が IPC を通じて戻る
10. App.tsx が refreshDocuments() を呼び出して一覧を更新する
11. DocumentList が新しいドキュメントで再描画される
```

## コンテンツ取得フロー

ドキュメント内容の表示には、専用の IPC チャンネルが追加されています。

```
1. ユーザーが DocumentDetail の "View Content" をクリックする
2. DocumentDetail が window.knowledgeBase.documents.getContent(id) を呼び出す
3. preload が 'documents:get-content' IPC を呼び出す
4. ipc-handlers が DocumentService.getDocumentContent(id) に処理を委譲する
5. PersistenceService が content/<id>.txt を読み取る
6. コンテンツが renderer に戻り、pre-wrap コンテナ内に表示される
```

## データ保存

```
knowledge-base-data/
  documents-meta.json     # ドキュメントメタデータ配列
  content/
    <doc-id>.txt          # 各ドキュメントから抽出したテキスト内容
  chunks/
    <doc-id>.json         # 各ドキュメントのチャンク配列
  index/
    index-meta.json       # ドキュメント ID とチャンク ID の対応表
  qa-history.json         # Q&A の対話ログ
```
