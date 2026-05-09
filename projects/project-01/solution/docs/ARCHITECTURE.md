# アーキテクチャ -- Knowledge Base Electron App

## システム概要

Knowledge Base は、TypeScript と React で構築された Electron デスクトップアプリケーションです。ドキュメントの取り込み、チャンク分割を伴うテキストインデックス化、そして引用付きの根拠に基づく質問応答を提供します。

## レイヤー図

```
+-----------------------------------------------------------+
|                     Renderer (React)                       |
|  App.tsx -> DocumentList, DocumentDetail, QuestionPanel,  |
|             StatusBar, ImportPanel                         |
+-----------------------------------------------------------+
         |  window.knowledgeBase.*（型付き IPC ブリッジ）
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
         |  サービスメソッド呼び出し
+-----------------------------------------------------------+
|                     Services レイヤー                    |
|  DocumentService | IndexingService | QaService             |
|  PersistenceService（filesystem I/O）                     |
+-----------------------------------------------------------+
```

## Electron のレイヤー

### Main Process (`src/main/`)

Main Process は、アプリケーションのライフサイクルを管理する Node.js プロセスです。責務は次のとおりです。

- **Window management**: `contextIsolation: true` と `nodeIntegration: false` を指定した安全な web preferences で `BrowserWindow` インスタンスを作成します。
- **IPC registration**: `registerIpcHandlers()` を通じて IPC チャネル名をサービスメソッドに対応付けます。
- **Service initialization**: 依存性注入を使って `PersistenceService`、`DocumentService`、`IndexingService`、`QaService` を構築します。

**重要な不変条件**: Main Process は React や renderer のコードを一切 import しません。

### Preload (`src/preload/`)

Preload スクリプトは、ページのスクリプトが読み込まれる前に renderer コンテキストで実行されます。Electron の `contextBridge` を使って、制限された型付き API を公開します。

```typescript
window.knowledgeBase = {
  documents: { list, import, get, delete },
  indexing:   { start, status, chunks },
  qa:         { ask, history },
}
```

**重要な不変条件**: Preload ブリッジは、renderer と main の唯一の通信経路です。renderer からは Node.js モジュールにアクセスできません。

### Renderer (`src/renderer/`)

Renderer は、Vite によってバンドルされる React 18 アプリケーションです。コンポーネントは次のとおりです。

- `App.tsx` -- ヘッダー、サイドバー、メインパネル、ステータスバーを備えたルートレイアウト。
- `DocumentList` -- 取り込んだドキュメントのサイドバー一覧。
- `DocumentDetail` -- ドキュメントのメタデータ、チャンク、インデックス操作を表示します。
- `ImportPanel` -- .txt と .md ドキュメントを取り込むためのファイル入力。
- `QuestionPanel` -- 質問を入力するためのテキスト入力欄。
- `StatusBar` -- インデックス状態とドキュメント数を表示します。

**重要な不変条件**: Renderer のコードは `fs`、`path`、`electron`、その他いかなる Node.js モジュールも import しません。

### Services (`src/services/`)

Main Process で動作するビジネスロジックのクラス群です。

- `PersistenceService` -- 原子的な書き込みを伴う低レベルの JSON/テキストファイル I/O。
- `DocumentService` -- ドキュメントの CRUD 操作（import, list, get, update, delete）。
- `IndexingService` -- 段落を考慮したチャンク分割（1チャンクあたり約 500 文字）とインデックス管理。
- `QaService` -- キーワードベースの検索と引用生成を行うモックの質問応答。

**重要な不変条件**: Services は共有型のみ import でき、renderer コードは決して import しません。

## データフロー

1. ユーザーが React コンポーネントとやり取りします（例: "Ask" をクリックする）。
2. コンポーネントが `window.knowledgeBase.qa.ask(question)` を呼び出します。
3. Preload ブリッジが `ipcRenderer.invoke('qa:ask', question)` を実行します。
4. Main Process の IPC ハンドラが `QaService.ask()` に処理を委譲します。
5. QaService がチャンクを取得し、キーワードの重なりでスコアを付け、回答を生成します。
6. 応答は IPC を通じて renderer に戻ります。
7. React コンポーネントが state を更新し、再レンダリングします。

## ビルドパイプライン

1. `tsc -p tsconfig.node.json` が main、preload、shared、services を `dist/` にコンパイルします。
2. `vite build` が renderer の React アプリを `dist/renderer/` にバンドルします。
3. Electron が `dist/main/main.js` をエントリーポイントとして読み込みます。

## データ保存

すべてのユーザーデータは `app.getPath('userData')/knowledge-base-data/` 以下に保存されます。

```
knowledge-base-data/
  documents-meta.json     # ドキュメントのメタデータ配列
  content/
    <doc-id>.txt          # 各ドキュメントから抽出したテキスト内容
  chunks/
    <doc-id>.json         # 各ドキュメントごとのチャンク配列
  index/
    index-meta.json       # ドキュメント ID とチャンク ID の対応表
  qa-history.json         # Q&A の対話ログ
```
