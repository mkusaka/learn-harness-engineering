# Architecture

## Layer Overview

Knowledge Base アプリケーションは、4つの主要レイヤーからなる厳格な
レイヤーアーキテクチャに従っています。各レイヤーには明確に定義された
責務と境界があり、それを越えてはなりません。

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

## Layer Boundaries

### Renderer Layer (`src/renderer/`)

**Responsibilities:**
- React を使って UI コンポーネントを描画する
- ユーザー入力を処理し、結果を表示する
- `window.knowledgeBase` API を通じてのみ main process と通信する

**Constraints:**
- `fs`, `path`, `os`, `child_process`、または任意の Node.js core module を import してはならない
- Electron APIs に直接アクセスしてはならない
- 表示用の整形を超える business logic や data transformation を含んではならない
- すべての data access は preload bridge を経由する

### Preload Layer (`src/preload/`)

**Responsibilities:**
- `contextBridge.exposeInMainWorld` を通じて、型付き API を renderer に公開する
- IPC channel 名を型付きの関数シグネチャに対応付ける
- renderer と main process の間の security boundary として機能する

**Constraints:**
- business logic を含んではならない
- services を直接 import してはならない
- 通信には `ipcRenderer.invoke` のみを使用する

### Main Process (`src/main/`)

**Responsibilities:**
- `BrowserWindow` インスタンスを作成・管理する
- services に委譲する IPC handler を登録する
- services を初期化し、そのライフサイクルを管理する
- application lifecycle events (`ready`, `activate`, `window-all-closed`) を処理する

**Constraints:**
- request routing を超える business logic を含んではならない
- すべての処理を service classes に委譲する
- persistence layer に直接アクセスしない

### Services Layer (`src/services/`)

**Responsibilities:**
- すべての business logic（document management、indexing、Q&A）を実装する
- すべての filesystem operations で `PersistenceService` を使用する
- structured logging には `logger` を使用する

**Constraints:**
- Electron APIs（`ipcMain`、`BrowserWindow` など）を import してはならない
- React や renderer components を import してはならない
- すべての filesystem access は `PersistenceService` を経由する

## IPC Channels

すべての IPC 通信は、`src/shared/types.ts` で定義された名前付き channel を使用します:

| Channel | Direction | Purpose |
|---------|-----------|---------|
| `documents:list` | Renderer -> Main | List all documents |
| `documents:import` | Renderer -> Main | Import a file |
| `documents:get` | Renderer -> Main | Get document by ID |
| `documents:delete` | Renderer -> Main | Delete a document |
| `indexing:start` | Renderer -> Main | Start indexing |
| `indexing:status` | Renderer -> Main | Get indexing status |
| `indexing:chunks` | Renderer -> Main | Get chunks for document |
| `qa:ask` | Renderer -> Main | Ask a question |
| `qa:history` | Renderer -> Main | Get Q&A history |

## Data Flow

1. renderer でのユーザー操作が `window.knowledgeBase.*` の呼び出しを発生させる
2. preload bridge がその呼び出しを `ipcRenderer.invoke(channel, ...args)` に変換する
3. main process の IPC handler が呼び出しを受け取り、適切な service に委譲する
4. service が `PersistenceService` を使って storage 向けの business logic を実行する
5. 結果が IPC を通じて renderer に戻り、表示される

## Architecture Verification

`bash scripts/check-architecture.sh` を実行して、layer boundary の違反がないことを確認してください。
このスクリプトは次をチェックします:
- renderer code に `fs` または `path` の import がないこと
- service code に Electron IPC の import がないこと
- services または main process に React の import がないこと
