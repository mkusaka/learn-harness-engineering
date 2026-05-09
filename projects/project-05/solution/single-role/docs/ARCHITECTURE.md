# アーキテクチャ

## レイヤー概要

Knowledge Base アプリケーションは、4つの主要レイヤーからなる厳格なレイヤードアーキテクチャに従います。
各レイヤーには明確に定義された責務と境界があり、それを越えてはなりません。

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

## レイヤーの境界

### Renderer Layer (`src/renderer/`)

**責務:**
- React を使って UI コンポーネントを描画する
- ユーザー入力を処理し、結果を表示する
- `window.knowledgeBase` API を通じてのみ main process と通信する

**制約:**
- `fs`, `path`, `os`, `child_process`、または任意の Node.js core module を import してはならない
- Electron APIs に直接アクセスしてはならない
- 表示用の整形を超える business logic や data transformation を含めてはならない
- すべての data access は preload bridge 経由で行う

### Preload Layer (`src/preload/`)

**責務:**
- `contextBridge.exposeInMainWorld` を通じて renderer に型付き API を公開する
- IPC channel 名を型付きの関数シグネチャに対応付ける
- renderer と main process の間のセキュリティ境界として機能する

**制約:**
- business logic を含めてはならない
- services を直接 import してはならない
- 通信には `ipcRenderer.invoke` のみを使用する

### Main Process (`src/main/`)

**責務:**
- BrowserWindow インスタンスを作成・管理する
- services に処理を委譲する IPC handlers を登録する
- services を初期化し、ライフサイクルを管理する
- アプリケーションのライフサイクルイベント（ready、activate、window-all-closed）を処理する

**制約:**
- request routing を超える business logic を含めてはならない
- すべての処理を service classes に委譲する
- persistence layer に直接アクセスしない

### Services Layer (`src/services/`)

**責務:**
- すべての business logic（document management、indexing、Q&A）を実装する
- すべての filesystem 操作に `PersistenceService` を使う
- 構造化ログには `logger` を使う

**制約:**
- Electron APIs（`ipcMain`, `BrowserWindow` など）を import してはならない
- React や renderer components を import してはならない
- すべての filesystem access は `PersistenceService` 経由で行う

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
4. service は `PersistenceService` を使って storage を行い、business logic を実行する
5. 結果は IPC を通じて renderer に戻り、表示される

## Architecture Verification

`bash scripts/check-architecture.sh` を実行して、レイヤー境界の違反がないことを確認します。
このスクリプトは以下をチェックします:
- renderer code に `fs` や `path` の import がないこと
- service code に Electron IPC の import がないこと
- services や main process に React の import がないこと
