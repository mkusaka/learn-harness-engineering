# CLAUDE.md -- Claude Code の簡易リファレンス

## Project Overview

これは Electron + TypeScript + React で構成されたナレッジベースアプリケーションです。コードベースは main process、preload、renderer、services の 4 層で構成されています。

## Build & Run

```bash
npm install        # 依存関係をインストールする
npm run check      # 出力せずに型チェックする
npm run build      # main/preload をコンパイルし、renderer をバンドルする
npm run dev        # ビルドして Electron を起動する
```

## Key Files

| File | Purpose |
|------|---------|
| `src/main/main.ts` | Electron のエントリポイント、ウィンドウ生成、サービスの配線 |
| `src/main/ipc-handlers.ts` | IPC チャネルの登録 |
| `src/preload/preload.ts` | contextBridge API の公開 |
| `src/renderer/App.tsx` | ルート React コンポーネント |
| `src/services/*.ts` | ビジネスロジック（document、indexing、QA、persistence） |
| `src/shared/types.ts` | 共有型と IPC チャネル定数 |
| `feature_list.json` | pass/fail 状態付きの機能追跡 |

## Architecture Rules

- renderer は Node.js モジュールを import しない。
- main と renderer の通信はすべて IPC 経由で行う。
- services はコンストラクタ注入された `PersistenceService` を使う。
- IPC チャネル名は `src/shared/types.ts` に置く。

## How to Add a Feature

1. `src/shared/types.ts` で IPC チャネルを定義する。
2. `src/main/ipc-handlers.ts` にハンドラを追加する。
3. `src/preload/preload.ts` で API を公開する。
4. `src/renderer/types.d.ts` に型宣言を追加する。
5. `src/renderer/components/` で UI を作成する。
6. 結果を `feature_list.json` に反映する。

## Testing

```bash
npm test           # vitest のテストスイートを実行する
npm run test:watch # watch モードでテストを実行する
```
