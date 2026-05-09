# AGENTS.md -- Project 02: エージェント向けワークスペース

## 起動時のルール

コードを書く前に、次の手順を順番に実行してください。

1. **このファイルを最後まで読む。** このプロジェクトの境界と規約が定義されています。
2. **`docs/ARCHITECTURE.md` を読む。** Electron のレイヤー構成と import の流れを理解するためです。
3. **`docs/PRODUCT.md` を読む。** 機能要件を理解するためです。
4. **`npm install && npm run check` を実行する。** プロジェクトが問題なくビルドできることを確認するためです。
5. **`feature_list.json` を読む。** すべての機能の現在の状態を確認するためです。

## ドキュメント階層

`docs/` ディレクトリは、エージェントが読みやすいように整理されています。

```
docs/
  ARCHITECTURE.md   -- Electron のレイヤー、データフロー、import パイプライン
  PRODUCT.md        -- 機能要件とユーザー向けの挙動
```

新しい機能を追加するときは、コードを書く前に該当するドキュメントを更新してください。これにより、セッション間で何が変わったのかをエージェントが把握しやすくなります。

## Electron レイヤーの境界

### Main Process (`src/main/`)
- `BrowserWindow` のライフサイクルと IPC 登録を担当します。
- すべてのファイルシステムアクセスは、サービス経由でここで行われます。

### Preload (`src/preload/`)
- main と renderer をつなぐ、唯一のブリッジです。
- `contextBridge.exposeInMainWorld` を使って型付き API を公開します。

### Renderer (`src/renderer/`)
- React + TypeScript の UI レイヤーです。
- 通信は `window.knowledgeBase` API を通じてのみ行います。
- Node.js モジュールを直接 import してはいけません。

### Services (`src/services/`)
- main process 内の純粋な TypeScript ビジネスロジックです。
- `PersistenceService` はコンストラクタ注入します。

## 規約

- TypeScript は strict mode を使います。理由を説明するコメントなしに `any` を使ってはいけません。
- Named exports only.
- IPC チャンネルは `src/shared/types.ts` で一度だけ定義します。
- 新しい IPC チャンネルは `namespace:action` の形式に従います（例: `documents:get-content`）。

## 完了条件

機能が「完了」とみなされるのは、次の条件を満たしたときです。

1. TypeScript がエラーなくコンパイルできる（`npm run check`）。
2. アプリが起動し、ウィンドウが表示される。
3. 機能が `feature_list.json` に `status` `"pass"` と証跡つきで記載される。
4. コードが Electron のレイヤー境界を守っている。
5. 変更内容を反映して `docs/ARCHITECTURE.md` および/または `docs/PRODUCT.md` が更新されている。

## セッション引き継ぎ

作業を再開するときは、前回セッションの文脈を確認するために `session-handoff.md` を読んでください。セッションを終えるときは、次の内容を更新してください。

- 何を達成したか
- 何が残っているか
- 発生したブロッカーや下した判断
- 変更したファイル
