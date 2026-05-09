# ソフトウェア設計ノート

## アーキテクチャ概要

このナレッジベースアプリケーションは、責務を明確に分離したレイヤードアーキテクチャに従っています。システムは主に4つのレイヤー、つまり main process、preload scripts、renderer layer、services に分かれています。

## Main Process

main process は、ウィンドウ管理、IPC handler の登録、ライフサイクル管理を担当します。Electron アプリケーションのエントリーポイントとして機能し、OS と renderer process の間を調整します。

主な責務:
- BrowserWindow の作成と設定
- IPC channel の登録
- Service の初期化と dependency injection
- アプリケーションのライフサイクルイベント（ready、window-all-closed、activate）

## Preload Layer

preload script は、main process と renderer process の間をつなぐ安全な橋渡し役を担います。Electron の `contextBridge` を使い、Node.js への完全なアクセスを与えずに、型付き API を renderer に公開します。

公開される API は、次の3つの namespace に整理されています:
- `documents` - ドキュメント管理の CRUD 操作
- `indexing` - ドキュメントのチャンク分割と index 管理
- `qa` - citation 付きの質問応答

## Renderer Layer

renderer は、React と TypeScript を使ってユーザーインターフェースを構築します。Component は preload bridge API 経由でのみ通信し、Node.js API や filesystem に直接アクセスすることはありません。

## Services Layer

ビジネスロジックは、main process で動作する service class に実装されています:
- `PersistenceService` - filesystem の read/write 操作
- `DocumentService` - ドキュメントの import、storage、retrieval
- `IndexingService` - テキストの chunking と index の構築
- `QaService` - citation 対応の mock Q&A

## Data Flow

1. renderer でのユーザー操作が、preload bridge を介した IPC call を発生させる
2. main process の IPC handler が適切な service に処理を委譲する
3. service が persistence layer を使って business logic を実行する
4. 結果が IPC を通じて renderer に戻り、表示される
