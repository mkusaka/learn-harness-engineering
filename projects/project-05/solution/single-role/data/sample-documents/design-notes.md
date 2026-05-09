# ソフトウェア設計ノート

## アーキテクチャ概要

このナレッジベースアプリケーションは、責務を明確に分離したレイヤー構成を採用しています。システムは、main process、preload scripts、renderer layer、services の 4 つの主要レイヤーに分かれています。

## Main Process

main process は、ウィンドウ管理、IPC ハンドラーの登録、ライフサイクル管理を担当します。Electron アプリケーションのエントリーポイントとして機能し、オペレーティングシステムと renderer process の間を調整します。

主な責務:
- `BrowserWindow` の作成と設定
- IPC チャンネルの登録
- サービスの初期化と依存性注入
- アプリケーションのライフサイクルイベント (`ready`、`window-all-closed`、`activate`)

## Preload Layer

preload script は、main process と renderer process の間にある安全なブリッジとして機能します。Electron の `contextBridge` を使って、完全な Node.js アクセス権を与えずに型付き API を renderer に公開します。

公開される API は 3 つの namespace に整理されています:
- `documents` - ドキュメント管理の CRUD 操作
- `indexing` - ドキュメントのチャンク分割と index 管理
- `qa` - 引用付きの質問応答

## Renderer Layer

renderer は、React と TypeScript を使ってユーザーインターフェースを構築します。コンポーネントは preload bridge API を通じてのみ通信し、Node.js API やファイルシステムに直接アクセスすることはありません。

## Services Layer

ビジネスロジックは、main process で動作する service クラスに配置されています:
- `PersistenceService` - ファイルシステムの読み書き操作
- `DocumentService` - ドキュメントの取り込み、保存、取得
- `IndexingService` - テキストのチャンク分割と index の構築
- `QaService` - 引用対応のモック Q&A

## データフロー

1. renderer でのユーザー操作が preload bridge 経由で IPC 呼び出しを発生させる
2. main process の IPC ハンドラーが適切な service に処理を委譲する
3. service が persistence layer を使ってビジネスロジックを実行する
4. 結果が IPC を通じて renderer に戻り、表示される
