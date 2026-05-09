# AGENTS.md -- Project 02: エージェント向けワークスペース

## クイックスタート

1. ビルドを確認するために `npm install && npm run check` を実行してください。
2. レイヤー構成については `docs/ARCHITECTURE.md` を読んでください。
3. 何を行う必要があるかは `feature_list.json` を確認してください。

## レイヤー

- Main process: `src/main/` -- ウィンドウ、IPC、サービス
- Preload: `src/preload/` -- ブリッジ API
- Renderer: `src/renderer/` -- React UI
- Services: `src/services/` -- ビジネスロジック

## 規約

- TypeScript の strict mode を使用します。コメントなしで `any` を使わないでください。
- named export のみを使用します。
- IPC チャンネルは `src/shared/types.ts` にあります。
