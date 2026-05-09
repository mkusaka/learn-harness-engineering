# AGENTS.md -- Project 03: スコープ管理と根拠に基づく検証

## 起動ルール

コードを書き始める前に、次の手順を順番に完了してください。

1. **このファイルを最後まで読む。** このプロジェクトの境界と規約が定義されています。
2. **`docs/ARCHITECTURE.md` を読む。** Electron のレイヤー構造、チャンク分割、Q&A の流れを理解してください。
3. **`docs/PRODUCT.md` を読む。** 機能要件を理解してください。
4. **`npm install && npm run check` を実行する。** プロジェクトが問題なくビルドできることを確認してください。
5. **`feature_list.json` を読む。** すべての機能の現在状態を確認してください。

## 1機能ずつ進める方針

**これが Project 03 の中核となる規律です。**

機能を実装するときは、必ず次の手順に従ってください。

1. **`feature_list.json` から、状態が `"not-started"` の機能をちょうど 1 つ選ぶ。**
2. **その機能だけを実装する。** 選んだ機能と無関係なコードは触らないでください。
3. **`npm run check` を実行し、動作をテストして** 機能が正しく動くことを確認する。
4. **`feature_list.json` を更新する。** 機能の状態を `"pass"` にし、根拠を追加してください。
5. **変更をコミットする。** コミットメッセージには機能 ID を含めてください。
6. **その後にだけ** 次の機能へ進む。

この方針に違反すること、つまり 1 回で複数の機能を実装したり、現在の機能の範囲外のファイルを編集したりすることは、このプロジェクトで最も一般的なバグと回帰の原因です。

### 機能の依存関係

```
metadata-extraction  -->  document-chunking  -->  indexing-status-ui
                                                  |
                                                  v
                                           grounded-qa
```

- `metadata-extraction` は `document-chunking` より先に完了していなければなりません（チャンクにはメタデータが必要です）。
- `document-chunking` は `indexing-status-ui` より先に完了していなければなりません（ステータスはチャンクを追跡します）。
- `document-chunking` は `grounded-qa` より先に完了していなければなりません（Q&A にはインデックス済みチャンクが必要です）。
- `indexing-status-ui` と `grounded-qa` は、チャンク分割の後であればどちらの順序でも実行できます。

## ドキュメント構成

`docs/` ディレクトリは、エージェントが読みやすいように整理されています。

```
docs/
  ARCHITECTURE.md   -- Electron layers, data flow, chunking pipeline, Q&A flow
  PRODUCT.md        -- Feature requirements and user-facing behavior
```

新しい機能を追加するときは、コードを書く前に関連するドキュメントを更新してください。

## Electron のレイヤー境界

### Main Process (`src/main/`)
- `BrowserWindow` のライフサイクルと IPC 登録を管理します。
- すべてのファイルシステムアクセスは、サービス経由でここで行われます。

### Preload (`src/preload/`)
- Main と renderer をつなぐ唯一の橋渡しです。
- `contextBridge.exposeInMainWorld` を使って型付き API を公開します。

### Renderer (`src/renderer/`)
- React + TypeScript の UI レイヤーです。
- 通信は `window.knowledgeBase` API 経由に限定されます。
- Node.js モジュールは一切 import しません。

### Services (`src/services/`)
- Main process 内の純粋な TypeScript のビジネスロジックです。
- `PersistenceService` はコンストラクタ注入されます。

## 規約

- TypeScript は strict mode です。理由を説明するコメントなしで `any` を使わないでください。
- Named exports only.
- IPC チャンネルは `src/shared/types.ts` で 1 回だけ定義します。
- 新しい IPC チャンネルは `namespace:action` の形式に従ってください。

## クリーン状態チェックリスト

プロジェクト完了を宣言する前に、`clean-state-checklist.md` の全項目を確認してください。

## セッション引き継ぎ

作業を再開するときは、前回セッションの文脈を確認するために `session-handoff.md` を読んでください。セッションを終えるときは、次の内容を追記してください。

- 実施した内容
- 残っている作業
- 発生した blocker または行った判断
- 変更したファイル
