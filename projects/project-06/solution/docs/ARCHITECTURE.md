# アーキテクチャ -- Knowledge Base Electron App (Capstone)

## システム概要

Knowledge Base は、TypeScript と React で構築された Electron デスクトップアプリケーションです。ドキュメントの取り込み、チャンク分割を伴うテキスト索引化、コンテンツ閲覧、出典付きの根拠ある質問応答、フィードバック収集、会話履歴、構造化ログによる実行時の可観測性を提供します。

これは、Learn Harness Engineering コースのすべての機能を統合した総仕上げのプロジェクトです。

## レイヤー図

```
+-----------------------------------------------------------+
|                     レンダラー (React)                    |
|  App.tsx -> DocumentList, DocumentDetail, ImportPanel,    |
|             QuestionPanel, ConversationHistory, StatusBar  |
+-----------------------------------------------------------+
         |  window.knowledgeBase.* (型付き IPC ブリッジ)
+-----------------------------------------------------------+
|                     プリロードスクリプト                   |
|  contextBridge.exposeInMainWorld ->                       |
|    documents, indexing, qa, feedback, app                  |
+-----------------------------------------------------------+
         |  ipcRenderer.invoke(IPC_CHANNELS.*)
+-----------------------------------------------------------+
|                     メインプロセス                        |
|  main.ts -> createWindow(), initializeServices()          |
|  ipc-handlers.ts -> registerIpcHandlers() (14 チャンネル) |
+-----------------------------------------------------------+
         |  サービスメソッド呼び出し
+-----------------------------------------------------------+
|                     サービス層                            |
|  DocumentService | IndexingService | QaService             |
|  PersistenceService (ファイルシステム I/O)                 |
|  Logger (構造化 JSON 出力)                                 |
+-----------------------------------------------------------+
```

## Electron のレイヤー

### Main Process (`src/main/`)

- **ウィンドウ管理**: 安全な web preferences を設定した `BrowserWindow` インスタンスを作成します。
- **IPC 登録**: `registerIpcHandlers()` を通じて 14 個の IPC チャンネル名をサービスメソッドに対応付けます。
- **サービス初期化**: 依存性注入を使ってすべてのサービスを構築します。
- **ログ出力**: アプリケーションのライフサイクルイベント（ready, shutdown）を記録します。

### Preload (`src/preload/`)

preload スクリプトは、`contextBridge` 経由で型付き API を公開します。

```typescript
window.knowledgeBase = {
  documents: { list, import, get, delete },
  indexing:   { start, status, chunks },
  qa:         { ask, history, clearHistory },
  feedback:   { submit, list },
  app:        { resetData },
}
```

### Renderer (`src/renderer/`)

Vite でバンドルされる React 18 アプリケーションです。

- `App.tsx` -- 表示モード切り替え（documents/history）と reset ボタンを備えたルートレイアウト。
- `DocumentList` -- 状態とチャンク数を含めて取り込んだドキュメントを一覧表示するサイドバー。
- `DocumentDetail` -- メタデータ、チャンク、索引作成の操作、削除ボタンを表示します。
- `ImportPanel` -- `.txt` と `.md` ドキュメントを取り込むためのファイル入力。
- `QuestionPanel` -- 質問入力用のテキストボックス。
- `ConversationHistory` -- 展開可能な引用、信頼度インジケータ、フィードバックボタン、履歴クリアを備えたチャット形式の Q&A 履歴。
- `StatusBar` -- インデックス状態、ドキュメント数、索引済み件数、最新のアクティビティを表示します。

### Services (`src/services/`)

- `Logger` -- DEBUG/INFO/WARN/ERROR レベル、サービスタグ、データペイロードを備えた構造化 JSON ログ。
- `PersistenceService` -- 原子的な書き込みとリセット機能を備えた低レベルの JSON/テキストファイル I/O。
- `DocumentService` -- バリデーション、コンテンツ保存、クリーンアップを含むドキュメント CRUD。
- `IndexingService` -- 段落を意識したチャンク分割（約 500 文字）、インデックス管理、メトリクス。
- `QaService` -- キーワード検索、引用、フィードバック、処理時間計測を備えたモック Q&A。

## 全体のデータフロー

### ドキュメント取り込みフロー

```
1. ユーザーが ImportPanel でファイルを選択します
2. App.tsx が `window.knowledgeBase.documents.import(filePath)` を呼び出します
3. Preload ブリッジが `ipcRenderer.invoke('documents:import', filePath)` を実行します
4. `ipc-handlers.ts` が `DocumentService.importDocument(filePath)` に処理を委譲します
5. DocumentService:
   a. ファイルが存在し、10MB 未満であることを検証します
   b. ファイル内容と統計情報を読み取ります
   c. UUID 付きの Document メタデータオブジェクトを作成します
   d. `PersistenceService` を使ってファイルを documents ディレクトリへコピーします
   e. 抽出したテキスト内容を `PersistenceService` を使って保存します
   f. `documents-meta.json` に追記します
   g. `documentId`、`filename`、`size` を含む構造化イベントをログ出力します
6. 結果は IPC を通じて戻ります
7. App.tsx が `refreshDocuments()` を呼び出して一覧を更新します
```

### 可観測性を伴う Q&A フロー

```
1. ユーザーが QuestionPanel に質問を入力します
2. App.tsx が `window.knowledgeBase.qa.ask(question)` を呼び出します
3. QaService.ask():
   a. 質問とその長さをログに記録します
   b. 処理遅延（100〜500ms）をシミュレートします
   c. `IndexingService` からすべてのチャンクを取得します
   d. 質問をキーワード（長さ 2 文字超）にトークナイズします
   e. キーワードの重なり数で各チャンクを採点します
   f. 上位 2 件のチャンクを引用として選択します
   g. モックパターンまたはフォールバックから回答を生成します
   h. 信頼度スコア付きの `QAResponse` を作成します
   i. `qa-history.json` に保存します
   j. `confidence`、`citationCount`、`durationMs` を含めて回答をログ出力します
4. 結果は renderer に渡されます
5. App.tsx が引用とフィードバックボタン付きで回答を表示します
```

### フィードバックフロー

```
1. ユーザーが回答に対して thumbs up/down をクリックします
2. App.tsx が `window.knowledgeBase.feedback.submit(timestamp, question, rating)` を呼び出します
3. QaService.submitFeedback():
   a. UUID、timestamp、rating を持つ `FeedbackEntry` を作成します
   b. `feedback.json` に追記します
   c. 構造化イベントをログ出力します
4. フィードバックはセッションをまたいで保持されます
```

### クリーンな状態へのリセットフロー

```
1. ユーザーがヘッダーのリセットボタンをクリックします
2. 確認ダイアログが表示されます
3. App.tsx が `window.knowledgeBase.app.resetData()` を呼び出します
4. PersistenceService.resetAll():
   a. データディレクトリ全体を削除します（`rmSync` の再帰削除）
   b. ディレクトリ構造を再作成します
   c. リセットイベントをログ出力します
5. App.tsx がすべての React state をクリアします
6. `refreshDocuments()` が空の状態を再読み込みします
```

## IPC チャンネル（全 14 件）

| チャンネル | 方向 | ハンドラ | 目的 |
|-----------|------|---------|------|
| `documents:list` | R -> M | DocumentService.listDocuments | すべてのドキュメントを一覧表示 |
| `documents:import` | R -> M | DocumentService.importDocument | ファイルを取り込む |
| `documents:get` | R -> M | DocumentService.getDocument | ID でドキュメントを取得 |
| `documents:delete` | R -> M | DocumentService.deleteDocument | ドキュメントを削除 |
| `indexing:start` | R -> M | IndexingService.startIndexing | 索引作成を開始 |
| `indexing:status` | R -> M | IndexingService.getStatus | 索引状態を取得 |
| `indexing:chunks` | R -> M | IndexingService.getChunksForDocument | チャンクを取得 |
| `qa:ask` | R -> M | QaService.ask | 質問する |
| `qa:history` | R -> M | QaService.getHistory | Q&A 履歴を取得 |
| `qa:clear-history` | R -> M | QaService.clearHistory | 履歴を消去 |
| `feedback:submit` | R -> M | QaService.submitFeedback | フィードバックを送信 |
| `feedback:list` | R -> M | QaService.getFeedback | すべてのフィードバックを取得 |
| `app:reset` | R -> M | PersistenceService.resetAll | すべてのデータをリセット |
| `app:status` | R -> M | IndexingService.getStatus | アプリ状態を取得 |

## データ保存

```
knowledge-base-data/
  documents-meta.json     # Document メタデータ配列
  content/
    <doc-id>.txt          # 各ドキュメントごとの抽出済みテキスト内容
  documents/
    <filename>            # 元ファイルのコピー
  chunks/
    <doc-id>.json         # 各ドキュメントごとのチャンク配列
  index/
    index-meta.json       # ドキュメント ID とチャンク ID の対応表
  qa-history.json         # Q&A のやり取りログ
  feedback.json           # フィードバックエントリ
```

## ロギング

すべてのログエントリは次の JSON 構造に従います。

```json
{
  "timestamp": "2026-03-30T12:00:00.000Z",
  "level": "INFO",
  "service": "document-service",
  "message": "Document imported successfully",
  "data": {
    "documentId": "abc-123",
    "filename": "design-notes.md",
    "sizeBytes": 2048,
    "contentLength": 1980,
    "totalDocuments": 3
  }
}
```

ログレベル:
- **DEBUG**: 日常的なデータアクセス（一覧取得、ファイル読み込み、チャンク取得）
- **INFO**: 重要なイベント（取り込み、索引化、Q&A、フィードバック、リセット）
- **WARN**: 不足しているが致命的ではないデータ（スキップされたドキュメント、コンテンツ未検出）
- **ERROR**: 失敗（ファイル未検出、解析エラー）
