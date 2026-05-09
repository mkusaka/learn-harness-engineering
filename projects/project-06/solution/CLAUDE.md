# CLAUDE.md -- Claude Code のクイックリファレンス

## Project Overview

これは、完全な可観測性、フィードバック、ベンチマーク機能を備えた、総仕上げとなる Electron + TypeScript + React のナレッジベースアプリケーションです。Learn Harness Engineering コースのすべての機能を統合しています。

## Build & Run

```bash
npm install        # 依存関係をインストール
npm run check      # 出力せずに型チェック
npm run build      # main/preload をコンパイル + renderer をバンドル
npm run dev        # ビルドして Electron を起動
npm test           # vitest スイートを実行
```

## Quick Start

```bash
bash init.sh       # 完全検証: install, check, build
```

## Key Files

| File | Purpose |
|------|---------|
| `src/main/main.ts` | Electron のエントリーポイント、ウィンドウ作成、サービス配線 |
| `src/main/ipc-handlers.ts` | IPC チャンネル登録 (14 チャンネル) |
| `src/preload/preload.ts` | contextBridge API (5 namespaces) |
| `src/renderer/App.tsx` | 画面切り替えを担うルート React コンポーネント |
| `src/renderer/components/ConversationHistory.tsx` | フィードバック付きのチャット形式 Q&A 履歴 |
| `src/services/logger.ts` | ログレベル付きの構造化 JSON ロギング |
| `src/services/persistence-service.ts` | ロギング付きのファイル I/O |
| `src/services/document-service.ts` | バリデーション付きの Document CRUD |
| `src/services/indexing-service.ts` | メトリクスログ付きのチャンク処理 |
| `src/services/qa-service.ts` | 引用とフィードバック付きの Q&A |
| `src/shared/types.ts` | Shared types and IPC channel constants |
| `feature_list.json` | 合否ステータスと証跡を持つ機能トラッキング |
| `scripts/benchmark.sh` | パフォーマンスベンチマークスイート |
| `scripts/cleanup-scanner.sh` | 古い成果物の検出 |

## Architecture Rules

- Renderer は Node.js モジュールを一切 import しない。
- main-renderer 間の通信はすべて IPC を通す。
- Services は constructor で注入された `PersistenceService` を使う。
- IPC チャンネル名は `src/shared/types.ts` に置く。
- すべての Services は `logger.forService()` 経由で構造化 JSON ロギングを使う。

## IPC Channels (14 total)

| Channel | Direction | Purpose |
|---------|-----------|---------|
| `documents:list` | R -> M | すべての文書を一覧表示する |
| `documents:import` | R -> M | ファイルを取り込む |
| `documents:get` | R -> M | ID で文書を取得する |
| `documents:delete` | R -> M | 文書を削除する |
| `indexing:start` | R -> M | インデックス作成を開始する |
| `indexing:status` | R -> M | インデックス作成の状態を取得する |
| `indexing:chunks` | R -> M | 文書のチャンクを取得する |
| `qa:ask` | R -> M | 質問する |
| `qa:history` | R -> M | Q&A 履歴を取得する |
| `qa:clear-history` | R -> M | Q&A 履歴を消去する |
| `feedback:submit` | R -> M | フィードバックを送信する |
| `feedback:list` | R -> M | すべてのフィードバックを取得する |
| `app:reset` | R -> M | すべてのデータをリセットする |
| `app:status` | R -> M | アプリの状態を取得する |

## How to Add a Feature

1. `src/shared/types.ts` で IPC チャンネルを定義する。
2. `src/main/ipc-handlers.ts` にログ付きで handler を追加する。
3. `src/preload/preload.ts` で API を公開する。
4. `src/renderer/types.d.ts` に型定義を追加する。
5. `src/renderer/components/` で UI を作る。
6. Service メソッドにログ呼び出しを追加する。
7. 結果を `feature_list.json` に反映する。

## Testing

```bash
npm test           # vitest スイートを実行
npm run test:watch # watch モードでテストを実行
bash scripts/benchmark.sh  # パフォーマンスベンチマークを実行
bash scripts/cleanup-scanner.sh  # 古い成果物を確認
```
