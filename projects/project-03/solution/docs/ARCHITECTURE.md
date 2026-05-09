# アーキテクチャ -- Knowledge Base Electron App

## システム概要

Knowledge Base は、TypeScript と React で構築された Electron デスクトップアプリケーションです。ファイルピッカーによるドキュメントの取り込み、メタデータ抽出、段落を考慮したチャンク分割によるテキスト索引、コンテンツ表示、引用と信頼度スコア付きの根拠に基づく質問応答を提供します。

## レイヤー図

```
+-----------------------------------------------------------+
|                  Renderer (React)                         |
|  App.tsx -> DocumentList, DocumentDetail, ImportPanel,    |
|             QuestionPanel, StatusBar                       |
+-----------------------------------------------------------+
         |  window.knowledgeBase.* (型付き IPC ブリッジ)
+-----------------------------------------------------------+
|                    Preload Script                         |
|  contextBridge.exposeInMainWorld -> documents, indexing, qa|
+-----------------------------------------------------------+
         |  ipcRenderer.invoke(IPC_CHANNELS.*)
+-----------------------------------------------------------+
|                     Main Process                           |
|  main.ts -> createWindow(), initializeServices()          |
|  ipc-handlers.ts -> registerIpcHandlers()                  |
+-----------------------------------------------------------+
         |  サービスメソッド呼び出し
+-----------------------------------------------------------+
|                     Services Layer                         |
|  DocumentService | IndexingService | QaService             |
|  PersistenceService (filesystem I/O)                       |
+-----------------------------------------------------------+
```

## Electron のレイヤー

### Main Process (`src/main/`)

- **ウィンドウ管理**: セキュアな web preferences を設定した `BrowserWindow` インスタンスを作成します。
- **IPC 登録**: `registerIpcHandlers()` を通じて、IPC チャンネル名をサービスメソッドに対応付けます。
- **サービス初期化**: 依存性注入を使ってすべてのサービスを構築します。Q&A がチャンクを取得できるように、`IndexingService` は `ipc-handlers` と `QaService` の間で共有されます。

### Preload (`src/preload/`)

preload スクリプトは、`contextBridge` を通じて型付き API を公開します。

```typescript
window.knowledgeBase = {
  documents: { list, import, get, getContent, delete },
  indexing:   { start, status, chunks },
  qa:         { ask, history },
}
```

### Renderer (`src/renderer/`)

Vite でバンドルされる React 18 アプリケーションです。

- `App.tsx` -- インポート切り替え、ドキュメント選択、Q&A、ステータスポーリングを備えたルートレイアウトです。
- `DocumentList` -- インポート済みドキュメントを一覧表示するサイドバーです。
- `DocumentDetail` -- メタデータ（抽出された単語数・行数・段落数を含む）、全文、チャンク、インデックス操作を表示します。
- `ImportPanel` -- .txt と .md ドキュメントを取り込むためのファイル入力です。
- `QuestionPanel` -- 質問を入力するテキスト欄です。
- `StatusBar` -- インデックス状態（色分け）、ドキュメント数、インデックス済み数、合計チャンク数を表示します。

### Services (`src/services/`)

- `PersistenceService` -- アトミック書き込みを伴う低レベルな JSON / テキストファイル I/O を担当します。
- `DocumentService` -- コンテンツ保存、メタデータ抽出、クリーンアップを含むドキュメント CRUD を担当します。
- `IndexingService` -- 段落を考慮したチャンク分割（約 500 文字）とインデックス管理を担当します。
- `QaService` -- キーワードベースの検索と引用生成を行うモック Q&A を担当します。

## メタデータ抽出を伴うインポートフロー

```
1. ユーザーが App.tsx の "Import" ボタンをクリックする
2. ImportPanel がファイル入力を表示する
3. ユーザーが .txt または .md ファイルを選択する
4. ImportPanel が onImport(file.path) を呼び出す
5. App.tsx が window.knowledgeBase.documents.import(filePath) を呼び出す
6. preload ブリッジが ipcRenderer.invoke('documents:import', filePath) を実行する
7. ipc-handlers.ts が DocumentService.importDocument(filePath) に処理を委譲する
8. DocumentService:
   a. ファイルの存在を検証する
   b. ファイル内容と統計情報を読み取る
   c. wordCount、lineCount、fileType、paragraphCount、charCount のメタデータを抽出する
   d. 抽出したメタデータを含む Document メタデータオブジェクトを作成する
   e. PersistenceService を通じてファイルを documents ディレクトリへコピーする
   f. 抽出したテキスト内容を PersistenceService を通じて保存する
   g. documents-meta.json に追記する
9. 結果が IPC を通じて戻る
10. App.tsx が refreshDocuments() を呼び出して一覧を更新する
11. DocumentList が新しいドキュメントで再レンダリングされる
```

## チャンク分割パイプライン

インデックス作成パイプラインは、ドキュメントの内容を検索可能なチャンクに分割します。

```
1. ユーザーが DocumentDetail で "Index Document" をクリックする（またはアプリが全件をインデックスする）
2. IPC 呼び出し: indexing:start(documentId)
3. IndexingService.startIndexing(documentId):
   a. content/<docId>.txt から内容を読み込む
   b. 改行 2 つ分で分割する（段落境界）
   c. バッファが約 500 文字に達するまで短い段落を結合する
   d. charCount、wordCount のメタデータ付き Chunk オブジェクトを作成する
   e. chunks/<docId>.json にチャンクを書き込む
   f. index-meta.json を chunk ID で更新する
4. 更新された IndexStatus を renderer に返す
5. StatusBar が新しいインデックス状態を反映する
```

### チャンク分割アルゴリズム

```
function chunkDocument(documentId, content):
  paragraphs = split on /\n\s*\n/
  buffer = ''
  chunkIndex = 0
  
  for each paragraph:
    if buffer.length + paragraph.length > 500 AND buffer.length > 0:
      emit chunk(buffer, chunkIndex++)
      buffer = paragraph
    else:
      buffer += paragraph
  
  if buffer not empty:
    emit chunk(buffer, chunkIndex)
```

## 根拠付き Q&A フロー

Q&A サービスは関連チャンクを取得し、根拠に基づいた回答を生成します。

```
1. ユーザーが QuestionPanel に質問を入力し、"Ask" をクリックする
2. IPC 呼び出し: qa:ask(question)
3. QaService.ask(question):
   a. 処理遅延（100〜500ms）をシミュレートする
   b. IndexingService.getAllChunks() からすべてのチャンクを取得する
   c. 質問を単語にトークナイズする（3 文字未満を除外）
   d. キーワードの重なり数で各チャンクを採点する
   e. 上位 2 件のチャンクを引用として採用する
   f. 引用用にドキュメントタイトルを参照する
   g. モックのパターン、またはフォールバックから回答を生成する
   h. 信頼度を計算する（引用ありは 0.85、なしは 0.30）
   i. qa-history.json に保存する
4. QAResponse を renderer に返す
5. App.tsx が引用パネル付きで回答を表示する
```

### 引用フォーマット

各引用には次の情報が含まれます。
- `documentId` -- 元ドキュメントへの参照
- `documentTitle` -- 人間が読めるドキュメント名
- `chunkIndex` -- ドキュメント内でのチャンク位置
- `excerpt` -- チャンク内容の先頭 200 文字

## コンテンツ取得フロー

```
1. ユーザーが DocumentDetail で "View Content" をクリックする
2. DocumentDetail が window.knowledgeBase.documents.getContent(id) を呼び出す
3. preload が 'documents:get-content' の IPC を呼び出す
4. ipc-handlers が DocumentService.getDocumentContent(id) に処理を委譲する
5. PersistenceService が content/<id>.txt を読み込む
6. コンテンツが renderer に戻り、pre-wrap コンテナで表示される
```

## データ保存

すべてのユーザーデータは `app.getPath('userData')/knowledge-base-data/` に保存されます。

```
knowledge-base-data/
  documents-meta.json     # ドキュメントメタデータ配列（抽出したメタデータを含む）
  content/
    <doc-id>.txt          # ドキュメントごとの抽出済みテキスト内容
  chunks/
    <doc-id>.json         # ドキュメントごとのチャンク配列
  index/
    index-meta.json       # ドキュメント ID と chunk ID の対応表
  qa-history.json         # Q&A のやり取りログ
```

## ビルドパイプライン

1. `tsc -p tsconfig.node.json` が main、preload、shared、services を `dist/` にコンパイルします。
2. `vite build` が renderer の React アプリを `dist/renderer/` にバンドルします。
3. Electron は `dist/main/main.js` をエントリーポイントとして読み込みます。
