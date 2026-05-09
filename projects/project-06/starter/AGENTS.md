# AGENTS.md -- Project 06: ランタイムの可観測性とデバッグ (Capstone)

## 起動ルール

1. このファイルを読むこと。
2. `npm install && npm run check` を実行してビルドを確認すること。
3. アプリは `npm run dev` で起動できること。

## プロジェクト概要

これは Learn Harness Engineering コースの集大成プロジェクトです。これまでのプロジェクトの機能をすべて組み合わせています:
- ドキュメントの取り込み、インデックス化、Q&A
- 会話履歴ビュー
- 可観測性のための構造化ログ
- テストのためのクリーンな状態管理

## 実装内容

アプリケーションには次のものが必要です:
1. チャット形式で表示する、動作する `ConversationHistory` コンポーネント
2. Q&A の応答に対するフィードバックボタン（親指上げ/下げ）
3. すべてのサービスで使う構造化ログ
4. クリーンな状態リセット関数
5. パフォーマンス測定用のベンチマークスクリプト

## 規約

- TypeScript strict mode.
- Named exports only.
- IPC channels defined in `src/shared/types.ts`.
- Services use constructor-injected `PersistenceService`.
