# アーキテクチャ

## レイヤー概要

Knowledge Base アプリケーションは、4つの主要レイヤーからなる厳格なレイヤーアーキテクチャに従っています。各レイヤーには明確な責務と境界があり、それを越えてはいけません。

```
Renderer (React UI)
    |
    v
Preload (contextBridge)
    |
    v
Main Process (IPC Handlers)
    |
    v
Services (Business Logic)
    |
    v
Persistence (Filesystem)
```

## レイヤー境界

### Renderer Layer (`src/renderer/`)

**Responsibilities:**
- React を使って UI コンポーネントを描画する
- ユーザー入力を処理し、結果を表示する
- main process とは `window.knowledgeBase` API 経由でのみ通信する

**Constraints:**
- `fs`, `path`, `os`, `child_process`、または任意の Node.js コアモジュールを import してはならない
- Electron API に直接アクセスしてはならない
- 表示用の整形を超えるビジネスロジックやデータ変換を含めてはならない
- すべてのデータアクセスは preload bridge を通す

### Preload Layer (`src/preload/`)

**Responsibilities:**
- `contextBridge.exposeInMainWorld` を通じて、型付き API を renderer に公開する
- IPC チャンネル名を型付きの関数シグネチャに対応付ける
- renderer と main process の間のセキュリティ境界として機能する

**Constraints:**
- ビジネスロジックを含めてはならない
- services を直接 import してはならない
- 通信には `ipcRenderer.invoke` のみを使う

### Main Process (`src/main/`)

**Responsibilities:**
- `BrowserWindow` インスタンスを作成し、管理する
- services に処理を委譲する IPC ハンドラを登録する
- services を初期化し、そのライフサイクルを管理する
- アプリケーションのライフサイクルイベント（ready, activate, window-all-closed）を処理する

**Constraints:**
- リクエストのルーティング以外のビジネスロジックを含めてはならない
- すべての処理を service クラスに委譲する
- persistence layer に直接アクセスしない

### Services Layer (`src/services/`)

**Responsibilities:**
- すべてのビジネスロジック（document management, indexing, Q&A）を実装する
- すべての filesystem 操作で `PersistenceService` を使う
- 構造化ログ出力に `logger` を使う

**Constraints:**
- Electron API（`ipcMain`, `BrowserWindow`, など）を import してはならない
- React や renderer コンポーネントを import してはならない
- すべての filesystem アクセスは `PersistenceService` を経由する

## IPC Channels

すべての IPC 通信は、`src/shared/types.ts` で定義された名前付きチャネルを使います:

| Channel | Direction | Purpose |
|---------|-----------|---------|
| `documents:list` | Renderer -> Main | すべての document を一覧表示する |
| `documents:import` | Renderer -> Main | ファイルを import する |
| `documents:get` | Renderer -> Main | ID で document を取得する |
| `documents:delete` | Renderer -> Main | document を削除する |
| `indexing:start` | Renderer -> Main | indexing を開始する |
| `indexing:status` | Renderer -> Main | indexing の状態を取得する |
| `indexing:chunks` | Renderer -> Main | document の chunks を取得する |
| `qa:ask` | Renderer -> Main | 質問する |
| `qa:history` | Renderer -> Main | Q&A 履歴を取得する |

## データフロー

1. renderer 上でのユーザー操作が `window.knowledgeBase.*` の呼び出しを発生させる
2. preload bridge がその呼び出しを `ipcRenderer.invoke(channel, ...args)` に変換する
3. main process の IPC ハンドラが呼び出しを受け取り、適切な service に委譲する
4. service が `PersistenceService` を使って storage に対するビジネスロジックを実行する
5. 結果が IPC を通って renderer に戻り、表示される

## アーキテクチャ検証

`bash scripts/check-architecture.sh` を実行すると、レイヤー境界の違反がないことを検証できます。このスクリプトは次を確認します:
- renderer コードに `fs` または `path` の import がないこと
- service コードに Electron IPC の import がないこと
- services または main process に React の import がないこと
