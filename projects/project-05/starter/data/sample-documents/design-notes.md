# ソフトウェア設計ノート

## アーキテクチャ概要

このナレッジベースアプリケーションは、責務を明確に分離したレイヤードアーキテクチャパターンに従っています。システムは、main process、preload scripts、renderer layer、services の4つの主要レイヤーに分かれています。

## Main Process

main process は、ウィンドウ管理、IPC handler の登録、ライフサイクル管理を担当します。Electron アプリケーションのエントリーポイントとして機能し、OS と renderer process の間を調整します。

主な責務:
- BrowserWindow creation and configuration
- IPC channel registration
- Service initialization and dependency injection
- Application lifecycle events (ready, window-all-closed, activate)

## Preload Layer

preload script は、main process と renderer process の間で安全な橋渡し役を果たします。Electron の `contextBridge` を使って、Node.js への完全なアクセス権を与えずに型付き API を renderer に公開します。

公開される API は、次の3つの namespace に整理されています。
- `documents` - CRUD operations for document management
- `indexing` - Document chunking and index management
- `qa` - Question answering with citations

## Renderer Layer

renderer は、React と TypeScript を使ってユーザーインターフェースを構築します。コンポーネントは preload bridge API を通じてのみ通信し、Node.js API や filesystem に直接アクセスすることはありません。

## Services Layer

ビジネスロジックは、main process で動作する service class に実装されています:
- `PersistenceService` - Filesystem read/write operations
- `DocumentService` - Document import, storage, and retrieval
- `IndexingService` - Text chunking and index building
- `QaService` - Mock Q&A with citation support

## Data Flow

1. renderer でのユーザー操作が preload bridge 経由で IPC 呼び出しを発生させる
2. main process の IPC handler が適切な service に処理を委譲する
3. service が persistence layer を使ってビジネスロジックを実行する
4. 結果が IPC を通じて renderer に戻り、画面に表示される
