# ソフトウェア設計ノート

## アーキテクチャ概要

このナレッジベースアプリケーションは、関心の分離が明確なレイヤードアーキテクチャパターンに従っています。システムは、main process、preload scripts、renderer layer、services の4つの主要レイヤーに分かれています。

## Main Process

main process は、ウィンドウ管理、IPC handler の登録、ライフサイクル管理を担当します。Electron アプリケーションのエントリーポイントとして機能し、OS と renderer process の間を調整します。

主な責務:
- BrowserWindow の作成と設定
- IPC channel の登録
- Service の初期化と dependency injection
- アプリケーションのライフサイクルイベント (`ready`, `window-all-closed`, `activate`)

## Preload Layer

preload script は、main process と renderer process の間にある安全な橋渡し役として機能します。Electron の `contextBridge` を使って、Node.js への完全なアクセスを与えることなく、型付き API を renderer に公開します。

公開される API は、3つの namespace に整理されています:
- `documents` - ドキュメント管理の CRUD 操作
- `indexing` - ドキュメントのチャンク分割と index 管理
- `qa` - 引用付きの質問応答

## Renderer Layer

renderer では React と TypeScript を使ってユーザーインターフェースを構築します。コンポーネントは preload bridge API を通じてのみ通信し、Node.js API や filesystem に直接アクセスすることはありません。

## Services Layer

ビジネスロジックは、main process で動作する service class に実装されています:
- `PersistenceService` - Filesystem read/write operations
- `DocumentService` - Document import, storage, and retrieval
- `IndexingService` - Text chunking and index building
- `QaService` - Mock Q&A with citation support

## Data Flow

1. renderer でのユーザー操作が、preload bridge を介した IPC call をトリガーする
2. main process の IPC handler が適切な service に処理を委譲する
3. service が persistence layer を使ってビジネスロジックを実行する
4. 結果が IPC を通じて renderer に戻され、表示される
