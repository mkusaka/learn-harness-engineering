# ソフトウェア設計メモ

## アーキテクチャ概要

このナレッジベースアプリケーションは、責務が明確に分離されたレイヤードアーキテクチャを採用しています。システムは主に4つのレイヤー、つまり main process、preload scripts、renderer layer、services に分かれています。

## Main Process

main process は、ウィンドウ管理、IPC ハンドラの登録、ライフサイクル管理を担当します。Electron アプリケーションのエントリポイントとして機能し、オペレーティングシステムと renderer process の間を調整します。

主な責務:
- `BrowserWindow` の作成と設定
- IPC チャネルの登録
- サービスの初期化と依存性注入
- アプリケーションのライフサイクルイベント (`ready`, `window-all-closed`, `activate`)

## Preload Layer

preload script は、main process と renderer process の間にある安全な橋渡し役です。Electron の `contextBridge` を使って、Node.js への完全なアクセス権を与えずに型付き API を renderer に公開します。

公開される API は3つの名前空間に整理されています:
- `documents` - ドキュメント管理の CRUD 操作
- `indexing` - ドキュメントの分割とインデックス管理
- `qa` - 引用付きの質問応答

## Renderer Layer

renderer は、React と TypeScript を使ってユーザーインターフェースを構築します。コンポーネントは preload bridge API を通じてのみ通信し、Node.js API やファイルシステムへ直接アクセスすることはありません。

## Services Layer

ビジネスロジックは、main process で動作する service クラスに実装されています:
- `PersistenceService` - ファイルシステムの読み書き
- `DocumentService` - ドキュメントの取り込み、保存、取得
- `IndexingService` - テキストの分割とインデックス作成
- `QaService` - 引用対応のモック Q&A
- `Logger` - ランタイムの可観測性のための構造化 JSON ロギング

## Data Flow

1. renderer でのユーザー操作が、preload bridge 経由で IPC 呼び出しを発生させます
2. main process の IPC ハンドラが、適切な service に処理を委譲します
3. service が persistence layer を使ってビジネスロジックを実行します
4. 結果は IPC を通じて renderer に戻され、画面に表示されます
5. 各ステップは、デバッグ用に構造化 JSON でログ出力されます

## Observability

すべての service は、次の項目を含む構造化ログエントリを出力します:
- ISO 8601 形式のタイムスタンプ
- ログレベル (`DEBUG`, `INFO`, `WARN`, `ERROR`)
- service 名タグ
- 人間が読めるメッセージ
- 任意の構造化データペイロード

これにより、実行時のデバッグと、後からのアプリケーション動作分析が可能になります。
