# ソフトウェア設計メモ

## アーキテクチャ概要

このナレッジベースアプリケーションは、責務が明確に分かれたレイヤードアーキテクチャパターンに従っています。システムは、main process、preload scripts、renderer layer、services の 4 つの主要レイヤーに分かれています。

## Main Process

main process は、ウィンドウ管理、IPC handler の登録、ライフサイクル管理を担当します。Electron アプリケーションのエントリポイントとして機能し、オペレーティングシステムと renderer process の間を調整します。

主な責務:
- BrowserWindow の作成と設定
- IPC channel の登録
- service の初期化と依存性注入
- アプリケーションのライフサイクルイベント (`ready`, `window-all-closed`, `activate`)

## Preload Layer

preload script は、main process と renderer process の間をつなぐ安全なブリッジとして機能します。Electron の `contextBridge` を使い、Node.js への完全なアクセスを与えずに型付き API を renderer に公開します。

公開される API は 3 つの namespace に整理されています:
- `documents` - ドキュメント管理の CRUD 操作
- `indexing` - ドキュメントの chunking と index 管理
- `qa` - citation 付きの質問応答

## Renderer Layer

renderer は React と TypeScript を使ってユーザーインターフェースを構築します。コンポーネントは preload bridge API を通じてのみ通信し、Node.js API や filesystem に直接アクセスすることはありません。

## Services Layer

ビジネスロジックは main process 上で動作する service クラスにあります:
- `PersistenceService` - filesystem の読み書き操作
- `DocumentService` - ドキュメントの取り込み、保存、取得
- `IndexingService` - テキストの chunking と index の構築
- `QaService` - citation 対応のモック Q&A

## Data Flow

1. renderer でのユーザー操作が preload bridge 経由で IPC call を発生させる
2. main process の IPC handler が適切な service に処理を委譲する
3. service が persistence layer を使ってビジネスロジックを実行する
4. 結果が IPC を通じて renderer に戻り、表示される
