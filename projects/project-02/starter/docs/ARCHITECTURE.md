# Architecture -- Knowledge Base Electron App

## System Overview

Knowledge Base は、TypeScript と React で構築された Electron のデスクトップアプリです。文書の取り込み、チャンク分割を伴うテキスト索引化、引用付きの根拠ベース回答を提供します。

## Layer Diagram

```
+-----------------------------------------------------------+
|                     Renderer (React)                       |
|  App.tsx -> DocumentList, DocumentDetail, QuestionPanel,  |
|             StatusBar, ImportPanel                         |
+-----------------------------------------------------------+
         |  window.knowledgeBase.* (typed IPC bridge)
+-----------------------------------------------------------+
|                     Preload Script                         |
|  contextBridge.exposeInMainWorld -> documents, indexing, qa|
+-----------------------------------------------------------+
         |  ipcRenderer.invoke(IPC_CHANNELS.*)
+-----------------------------------------------------------+
|                     Main Process                           |
|  main.ts -> createWindow(), initializeServices()          |
|  ipc-handlers.ts -> registerIpcHandlers()                  |
+-----------------------------------------------------------+
         |  Service method calls
+-----------------------------------------------------------+
|                     Services Layer                         |
|  DocumentService | IndexingService | QaService             |
|  PersistenceService (filesystem I/O)                       |
+-----------------------------------------------------------+
```

## Electron Layers

### Main Process (`src/main/`)

main process は、アプリケーションのライフサイクルを管理する Node.js プロセスです。責務は次のとおりです。

- **Window management**: `contextIsolation: true`、`nodeIntegration: false` といった安全な web preferences を指定して `BrowserWindow` インスタンスを作成します。
- **IPC registration**: `registerIpcHandlers()` を通じて、IPC チャンネル名を service メソッドに対応付けます。
- **Service initialization**: 依存性注入を使って `PersistenceService`、`DocumentService`、`IndexingService`、`QaService` を構築します。

**Key invariant**: main process は React や renderer のコードを決して import しません。

### Preload (`src/preload/`)

preload script は、いずれの page scripts よりも先に renderer コンテキストで実行されます。Electron の `contextBridge` を使って、制限された型付き API を公開します。

```typescript
window.knowledgeBase = {
  documents: { list, import, get, delete },
  indexing:   { start, status, chunks },
  qa:         { ask, history },
}
```

**Key invariant**: renderer と main の通信チャネルは preload bridge だけです。renderer からは Node.js モジュールにアクセスできません。

### Renderer (`src/renderer/`)

renderer は、Vite でバンドルされる React 18 アプリケーションです。コンポーネントは次のとおりです。

- `App.tsx` -- ヘッダー、サイドバー、メインパネル、ステータスバーを備えたルートレイアウト。
- `DocumentList` -- 取り込まれた文書の一覧を表示するサイドバー。
- `DocumentDetail` -- 文書のメタデータ、チャンク、索引化コントロールを表示します。
- `ImportPanel` -- `.txt` と `.md` 文書を取り込むためのファイル入力。
- `QuestionPanel` -- 質問を送るためのテキスト入力。
- `StatusBar` -- 索引の状態と文書数を表示します。

**Key invariant**: renderer のコードは `fs`、`path`、`electron`、または他の Node.js モジュールを import しません。

### Services (`src/services/`)

main process で動作するビジネスロジックのクラス群です。

- `PersistenceService` -- 原子的書き込みを伴う低レベルの JSON/text ファイル I/O。
- `DocumentService` -- 文書の CRUD 操作（import、list、get、update、delete）。
- `IndexingService` -- 段落を意識したチャンク分割（1 チャンクあたり約 500 文字）と index 管理。
- `QaService` -- キーワードベースの検索と引用生成を行うモックの質問応答。

**Key invariant**: services は共有型を import してよいですが、renderer のコードは import しません。

## Data Flow

1. ユーザーが React コンポーネントを操作します（例: "Ask" をクリック）。
2. コンポーネントが `window.knowledgeBase.qa.ask(question)` を呼び出します。
3. preload bridge が `ipcRenderer.invoke('qa:ask', question)` を実行します。
4. main process の IPC handler が `QaService.ask()` に処理を委譲します。
5. `QaService` がチャンクを取得し、キーワードの重なりでスコア付けし、回答を生成します。
6. レスポンスは IPC を通じて renderer に戻ります。
7. React コンポーネントが state を更新し、再レンダーします。

## Build Pipeline

1. `tsc -p tsconfig.node.json` が main、preload、shared、services を `dist/` にコンパイルします。
2. `vite build` が renderer の React アプリを `dist/renderer/` にバンドルします。
3. Electron がエントリポイントとして `dist/main/main.js` を読み込みます。

## Data Storage

すべてのユーザーデータは `app.getPath('userData')/knowledge-base-data/` の下に保存されます。

```
knowledge-base-data/
  documents-meta.json     # Document metadata array
  content/
    <doc-id>.txt          # Extracted text content per document
  chunks/
    <doc-id>.json         # Chunk array per document
  index/
    index-meta.json       # Mapping of document IDs to chunk IDs
  qa-history.json         # Q&A interaction log
```
