# アーキテクチャ

## レイヤー概要

Knowledge Base アプリケーションは、4つの主要レイヤーから成る厳格な階層アーキテクチャに従っています。各レイヤーには明確な責務と境界があり、それを越えてはなりません。

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

### Renderer レイヤー (`src/renderer/`)

**責務:**
- React を使って UI コンポーネントを描画する
- ユーザー入力を処理し、結果を表示する
- main process とは `window.knowledgeBase` API を通じてのみ通信する

**制約:**
- `fs`, `path`, `os`, `child_process`、または任意の Node.js コアモジュールを **MUST NOT** import する
- Electron API を直接 **MUST NOT** 使う
- 表示用の整形を超えるビジネスロジックやデータ変換を **MUST NOT** 含める
- すべてのデータアクセスは preload bridge を経由する

### Preload レイヤー (`src/preload/`)

**責務:**
- `contextBridge.exposeInMainWorld` を通じて型付き API を renderer に公開する
- IPC チャンネル名を型付きの関数シグネチャに対応付ける
- renderer と main process の間のセキュリティ境界として機能する

**制約:**
- ビジネスロジックを **MUST NOT** 含める
- services を直接 import しない
- 通信には `ipcRenderer.invoke` のみを使う

### Main Process (`src/main/`)

**責務:**
- BrowserWindow インスタンスを作成し、管理する
- services に委譲する IPC ハンドラを登録する
- services を初期化し、そのライフサイクルを管理する
- アプリケーションのライフサイクルイベント（ready, activate, window-all-closed）を処理する

**制約:**
- リクエストルーティングを超えるビジネスロジックを **MUST NOT** 含める
- すべての処理を service クラスに委譲する
- persistence layer に直接アクセスしない

### Services レイヤー (`src/services/`)

**責務:**
- すべてのビジネスロジック（document management, indexing, Q&A）を実装する
- すべてのファイルシステム操作に `PersistenceService` を使う
- 構造化ログに `logger` を使う

**制約:**
- Electron API（`ipcMain`, `BrowserWindow`, etc.）を import しない
- React や renderer コンポーネントを import しない
- すべてのファイルシステムアクセスは `PersistenceService` を経由する

## IPC チャンネル

すべての IPC 通信は、`src/shared/types.ts` で定義された名前付きチャンネルを使います。

| Channel | Direction | Purpose |
|---------|-----------|---------|
| `documents:list` | Renderer -> Main | すべての document を一覧表示する |
| `documents:import` | Renderer -> Main | ファイルを import する |
| `documents:get` | Renderer -> Main | ID で document を取得する |
| `documents:delete` | Renderer -> Main | document を削除する |
| `indexing:start` | Renderer -> Main | indexing を開始する |
| `indexing:status` | Renderer -> Main | indexing の状態を取得する |
| `indexing:chunks` | Renderer -> Main | document の chunks を取得する |
| `qa:ask` | Renderer -> Main | 質問を送る |
| `qa:history` | Renderer -> Main | Q&A 履歴を取得する |

## データフロー

1. renderer でのユーザー操作が `window.knowledgeBase.*` への呼び出しを発生させる
2. preload bridge が呼び出しを `ipcRenderer.invoke(channel, ...args)` に変換する
3. main process の IPC ハンドラが呼び出しを受け取り、適切な service に委譲する
4. service が `PersistenceService` を使って保存処理を伴うビジネスロジックを実行する
5. 結果が IPC を通って renderer に戻り、表示される

## アーキテクチャ検証

`bash scripts/check-architecture.sh` を実行すると、レイヤー境界の違反がないことを確認できます。このスクリプトは次をチェックします。
- renderer コードに `fs` または `path` の import がないこと
- service コードに Electron IPC の import がないこと
- services または main process に React の import がないこと
