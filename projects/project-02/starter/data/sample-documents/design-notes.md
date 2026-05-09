# ソフトウェア設計メモ

## アーキテクチャ概要

このナレッジベースアプリケーションは、責務分離が明確なレイヤードアーキテクチャパターンに従っています。システムは、main process、preload scripts、renderer layer、services の4つの主要レイヤーに分かれています。

## Main Process

main process は、ウィンドウ管理、IPC ハンドラの登録、ライフサイクル管理を担当します。Electron アプリケーションのエントリーポイントとして機能し、オペレーティングシステムと renderer process の間を調整します。

主な責務:
- BrowserWindow の作成と設定
- IPC チャネルの登録
- サービスの初期化と依存性注入
- アプリケーションのライフサイクルイベント（ready、window-all-closed、activate）

## Preload Layer

preload script は、main process と renderer process の間をつなぐ安全な橋渡し役です。Electron の contextBridge を使って、Node.js への完全なアクセス権を与えることなく、型付き API を renderer に公開します。

公開される API は、次の3つの namespace に整理されています。
- `documents` - ドキュメント管理の CRUD 操作
- `indexing` - ドキュメントのチャンク化と index 管理
- `qa` - 引用付きの質問応答

## Renderer Layer

renderer は React と TypeScript を使ってユーザーインターフェースを構築します。コンポーネントは preload bridge API を通じてのみ通信し、Node.js API やファイルシステムに直接アクセスすることはありません。

## Services Layer

ビジネスロジックは、main process で動作する service class に実装されています。
- `PersistenceService` - ファイルシステムの読み書き操作
- `DocumentService` - ドキュメントの取り込み、保存、取得
- `IndexingService` - テキストのチャンク化と index の構築
- `QaService` - 引用対応のモック Q&A

## Data Flow

1. renderer でのユーザー操作が preload bridge 経由で IPC 呼び出しを発生させる
2. main process の IPC ハンドラが適切な service に処理を委譲する
3. service が persistence layer を使ってビジネスロジックを実行する
4. 結果が IPC を通じて renderer に戻り、表示される
