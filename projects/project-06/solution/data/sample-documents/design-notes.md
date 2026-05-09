# ソフトウェア設計メモ

## アーキテクチャ概要

このナレッジベースアプリケーションは、責務が明確に分離されたレイヤードアーキテクチャのパターンに従っています。システムは、main process、preload scripts、renderer layer、services の4つの主要レイヤーに分かれています。

## Main Process

main process は、ウィンドウ管理、IPC ハンドラーの登録、ライフサイクル管理を担当します。Electron アプリケーションのエントリポイントとして機能し、OS と renderer process の間を調整します。

主な責務:
- `BrowserWindow` の作成と設定
- IPC チャネルの登録
- service の初期化と dependency injection
- アプリケーションのライフサイクルイベント (`ready`, `window-all-closed`, `activate`)

## Preload Layer

preload script は、main process と renderer process の間にある安全なブリッジとして機能します。Electron の `contextBridge` を使って、完全な Node.js アクセスを与えることなく、型付き API を renderer に公開します。

公開される API は namespace ごとに整理されています:
- `documents` - 文書管理の CRUD 操作
- `indexing` - 文書のチャンク化と index 管理
- `qa` - 引用付きの質問応答
- `feedback` - Q&A 応答の評価
- `app` - アプリケーションレベルの操作 (`reset data`)

## Renderer Layer

renderer は、React と TypeScript を使ってユーザーインターフェースを構築します。コンポーネントは preload bridge API を通じてのみ通信し、Node.js API や filesystem に直接アクセスすることはありません。

主なコンポーネント:
- `App.tsx` - 表示モード切り替えを備えたルートレイアウト
- `DocumentList` - 文書を閲覧するためのサイドバー
- `DocumentDetail` - index 操作用コントロールを備えた文書ビューア
- `ConversationHistory` - フィードバック付きのチャット形式 Q&A 履歴
- `QuestionPanel` - 質問入力欄
- `StatusBar` - index 状態とメトリクス

## Services Layer

ビジネスロジックは、main process 上で動作する service class に実装されています:
- `PersistenceService` - JSON とテキストを扱う filesystem の読み書き操作
- `DocumentService` - 文書の取り込み、保存、取得、削除
- `IndexingService` - テキストのチャンク化、index の構築、状態追跡
- `QaService` - キーワード検索、引用、フィードバックを伴うモック Q&A
- `Logger` - 実行時の可観測性のための構造化 JSON ログ

## Data Flow

1. renderer でのユーザー操作が、preload bridge を通じて IPC 呼び出しを発生させる
2. main process の IPC ハンドラーが適切な service に処理を委譲する
3. service が persistence layer を使ってビジネスロジックを実行する
4. 各 service が、タイムスタンプとデータを含む構造化イベントをログに記録する
5. 結果が IPC を通じて renderer に戻り、画面に表示される

## Observability

すべての service は、次の情報を含む構造化 JSON ログエントリを出力します:
- ISO 8601 形式のタイムスタンプ
- ログレベル (`DEBUG`, `INFO`, `WARN`, `ERROR`)
- service 名のタグ
- 人間が読めるメッセージ
- 任意の構造化データペイロード

これにより、実行時デバッグとアプリケーション動作の事後分析が可能になります。
