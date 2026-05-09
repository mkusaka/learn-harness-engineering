# AGENTS.md -- Project 03: スコープ管理と根拠に基づく検証

## クイックスタート

1. ビルドを確認するために `npm install && npm run check` を実行してください。
2. レイヤー構成については `docs/ARCHITECTURE.md` を読んでください。
3. 機能要件については `docs/PRODUCT.md` を読んでください。
4. 何を進めるべきかは `feature_list.json` を確認してください。

## レイヤー

- Main: `src/main/` -- window, IPC, services
- Preload: `src/preload/` -- bridge API
- Renderer: `src/renderer/` -- React UI
- Services: `src/services/` -- business logic

## 規約

- TypeScript は strict mode です。`any` を使う場合はコメントを付けてください。
- export は named export のみを使ってください。
- IPC channels は `src/shared/types.ts` にあります。

## 実装対象の機能

このプロジェクトで新たに実装する機能は次のとおりです。

1. **Document Chunking** -- `IndexingService` が文書をおよそ500文字ごとの chunk に分割する
2. **Metadata Extraction** -- import 時に単語数、行数、ファイル種別を抽出する
3. **Indexing Status UI** -- `StatusBar` が件数付きで indexing の進捗を表示する
4. **Grounded Q&A** -- `QaService` が引用と confidence 付きで回答を返す

現在の状態は `feature_list.json` を確認してください。

## 完了条件

機能が「完了」とみなされるのは、次の条件を満たしたときです。

1. TypeScript がエラーなくコンパイルできること（`npm run check`）。
2. アプリが起動し、機能が動作すること。
3. `feature_list.json` に status `"pass"` と evidence を伴ってその機能が記載されていること。

## セッション引き継ぎ

作業を再開するときは、前回セッションの文脈として `session-handoff.md` を読んでください。
