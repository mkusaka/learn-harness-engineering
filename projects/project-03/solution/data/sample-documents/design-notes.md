# Software Design Notes

## Architecture Overview

ナレッジベースアプリケーションは、責務を明確に分離したレイヤー型アーキテクチャに従っています。システムは、main process、preload scripts、renderer layer、services の4つの主要なレイヤーに分かれています。

## Main Process

main process は、ウィンドウ管理、IPC handler の登録、ライフサイクル管理を担当します。Electron アプリケーションのエントリーポイントとして機能し、オペレーティングシステムと renderer process の間を調整します。

主な責務:
- `BrowserWindow` の作成と設定
- IPC channel の登録
- service の初期化と dependency injection
- アプリケーションのライフサイクルイベント (`ready`, `window-all-closed`, `activate`)

## Preload Layer

preload script は、main process と renderer process の間をつなぐ安全なブリッジとして機能します。Electron の `contextBridge` を使って、完全な Node.js アクセスを与えることなく、型付きの API を renderer に公開します。

公開される API は3つの namespace に整理されています:
- `documents` - document 管理の CRUD 操作
- `indexing` - document の chunking と index 管理
- `qa` - citation 付きの question answering

## Renderer Layer

renderer は React と TypeScript を使ってユーザーインターフェースを構築します。コンポーネント同士の通信は preload bridge API 経由に限定されており、Node.js API や filesystem に直接アクセスすることはありません。

## Services Layer

business logic は main process で動作する service class に実装されています:
- `PersistenceService` - filesystem の read/write 操作
- `DocumentService` - document の import、保存、取得
- `IndexingService` - text chunking と index の構築
- `QaService` - citation 対応の mock Q&A

## Data Flow

1. renderer でのユーザー操作が preload bridge 経由の IPC call を引き起こします
2. main process の IPC handler が適切な service に処理を委譲します
3. service が persistence layer を使って business logic を実行します
4. 結果が IPC を通じて renderer に戻り、表示されます
