# AGENTS.md -- Project 01: Baseline vs Minimal Harness

## 起動時のルール

コードを書く前に、次の手順を順番に実行してください。

1. **このファイルを最後まで読む。** このプロジェクトの境界と規約が定義されています。
2. **`docs/ARCHITECTURE.md` を読む。** Electron のレイヤー構成を理解してください。
3. **`docs/PRODUCT.md` を読む。** 機能要件を理解してください。
4. **`bash init.sh` を実行する。** プロジェクトが問題なくビルドできることを確認してください。失敗した場合は、先にビルドエラーを修正してください。
5. **`feature_list.json` を読む。** すべての機能の現在の状態を確認してください。

## Electron レイヤーの境界

このプロジェクトには、厳密に分けられた 4 つのレイヤーがあります。コードはこの境界を必ず守ってください。

### Main Process (`src/main/`)
- `BrowserWindow` のライフサイクルと IPC の登録を担当します。
- サービスは読み込みますが、renderer のコードは読み込みません。
- ファイルシステムへのアクセスは、すべてここでサービス経由で行います。

### Preload (`src/preload/`)
- main と renderer をつなぐ唯一の橋渡しです。
- `contextBridge.exposeInMainWorld` を使って型付き API を公開します。
- React や renderer のコードは読み込みません。

### Renderer (`src/renderer/`)
- React + TypeScript の UI レイヤーです。
- `window.knowledgeBase` API を通じてのみ main process と通信します。
- Node.js モジュール（`fs`, `path`, `electron`）は読み込みません。
- `types.d.ts` の型宣言を使います。

### Services (`src/services/`)
- main process で動作する、純粋な TypeScript のビジネスロジックです。
- Services は `src/shared/` からは読み込めますが、`src/renderer/` からは読み込めません。
- 各 service はコンストラクタ注入で `PersistenceService` を受け取ります。

## 規約

- TypeScript の strict mode が有効です。理由のコメントなしに `any` 型を使わないでください。
- 名前付き export を使ってください。default export は使いません。
- IPC channel 名は `src/shared/types.ts` の `IPC_CHANNELS` で 1 回だけ定義します。
- すべての非同期操作は Promise を返してください。renderer で同期 I/O を使ってはいけません。

## 完了条件

機能が「完了」とみなされるのは、次のすべてを満たしたときです。

1. TypeScript がエラーなしでコンパイルできること（`npm run check`）。
2. アプリが起動し、ウィンドウが表示されること（`npm run dev`）。
3. 機能が `feature_list.json` に `"pass"` 状態と根拠つきで載っていること。
4. コードが上記の Electron レイヤー境界を守っていること。
5. 通常操作中にコンソールエラーが出ないこと。

## Feature List の扱い

`feature_list.json` ファイルが、このプロジェクトの進捗に関する唯一の正本です。

- 各 feature には `"pass"`、`"fail"`、`"not-started"` のいずれかの `status` があります。
- feature を実装したら、根拠を添えて `status` を `"pass"` に更新してください。
- feature がブロックされている場合は、理由を添えて `status` を `"fail"` に設定してください。
- リストから feature を削除しないでください。
