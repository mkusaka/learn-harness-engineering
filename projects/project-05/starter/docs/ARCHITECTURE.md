# Architecture

## レイヤー概要

Knowledge Base アプリケーションは、4つの主要レイヤーからなる厳格なレイヤードアーキテクチャに従っています。各レイヤーには明確に定められた責務と境界があり、それを越えてはなりません。

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

**責務:**
- React を使って UI コンポーネントを描画する
- ユーザー入力を処理し、結果を表示する
- メインプロセスとは `window.knowledgeBase` API を通じてのみ通信する

**制約:**
- `fs`, `path`, `os`, `child_process`、またはその他の Node.js コアモジュールを import してはならない
- Electron API に直接アクセスしてはならない
- 表示用の整形を超えるビジネスロジックやデータ変換を含めてはならない
- すべてのデータアクセスは preload ブリッジ経由で行う

### Preload Layer (`src/preload/`)

**責務:**
- `contextBridge.exposeInMainWorld` を通じて型付き API を renderer に公開する
- IPC チャンネル名を型付きの関数シグネチャに対応付ける
- renderer と main process の間のセキュリティ境界として機能する

**制約:**
- ビジネスロジックを含めてはならない
- services を直接 import してはならない
- 通信には `ipcRenderer.invoke` だけを使用する

### Main Process (`src/main/`)

**責務:**
- BrowserWindow インスタンスを生成し、管理する
- services に処理を委譲する IPC ハンドラを登録する
- services を初期化し、そのライフサイクルを管理する
- アプリケーションのライフサイクルイベント (`ready`, `activate`, `window-all-closed`) を処理する

**制約:**
- リクエストのルーティングを超えるビジネスロジックを含めてはならない
- すべての処理を service クラスに委譲する
- persistence レイヤーに直接アクセスしない

### Services Layer (`src/services/`)

**責務:**
- すべてのビジネスロジック（文書管理、インデックス作成、Q&A）を実装する
- すべてのファイルシステム操作に `PersistenceService` を使用する
- 構造化ログ出力に `logger` を使用する

**制約:**
- Electron API (`ipcMain`, `BrowserWindow` など) を import してはならない
- React や renderer コンポーネントを import してはならない
- すべてのファイルシステムアクセスは `PersistenceService` 経由で行う

## IPC チャンネル

すべての IPC 通信は、`src/shared/types.ts` で定義された名前付きチャンネルを使用します:

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

## データフロー

1. renderer でのユーザー操作により `window.knowledgeBase.*` への呼び出しが発生する
2. preload ブリッジが呼び出しを `ipcRenderer.invoke(channel, ...args)` に変換する
3. main process の IPC ハンドラが呼び出しを受け取り、適切な service に委譲する
4. service が `PersistenceService` を使ってストレージ向けのビジネスロジックを実行する
5. 結果が IPC を通じて renderer に戻り、表示される

## アーキテクチャ検証

`bash scripts/check-architecture.sh` を実行すると、レイヤー境界の違反がないことを検証できます。このスクリプトは次を確認します:
- renderer コードに `fs` または `path` の import がないこと
- service コードに Electron IPC の import がないこと
- services または main process に React の import がないこと
