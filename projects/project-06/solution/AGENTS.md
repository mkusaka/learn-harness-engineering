# AGENTS.md -- Project 06: Runtime Observability and Debugging (Capstone)

## 起動時のルール

コードを書く前に、次の手順を順番に完了してください。

1. **このファイルを最後まで読む。** このプロジェクトの境界と慣例が定義されています。
2. Claude Code を使う場合は、簡易参照として **`CLAUDE.md` を読む。**
3. Electron レイヤーの全体構造とデータフローを理解するために **`docs/ARCHITECTURE.md` を読む。**
4. 完全な機能要件を理解するために **`docs/PRODUCT.md` を読む。**
5. ロギング、可観測性、クリーンな状態に関する要件を理解するために **`docs/RELIABILITY.md` を読む。**
6. プロジェクトが正しくビルドされ、初期化されることを確認するために **`bash init.sh` を実行する。**
7. すべての機能の現在の状態を確認するために **`feature_list.json` を読む。**

## プロジェクトの位置づけ

これは Learn Harness Engineering コースの **キャップストーンプロジェクト** です。Projects 01-05 のすべての機能を 1 つの完成した製品に統合しています。

- 検証付きのドキュメント取り込み
- 進捗追跡付きのテキスト索引化
- 引用付きの根拠ある Q&A
- チャット形式で表示される会話履歴
- 実行時の可観測性のための構造化ロギング
- Q&A 応答に対するフィードバック収集
- テスト用のクリーン状態リセット
- パフォーマンス測定用のベンチマークスクリプト
- 古い成果物を検出するクリーンアップスキャナー

## ドキュメント階層

`docs/` ディレクトリは、エージェントが読みやすいように整理されています。

```
docs/
  ARCHITECTURE.md   -- Electron レイヤー、データフロー、全体パイプライン
  PRODUCT.md        -- 機能要件とユーザー向けの振る舞い
  RELIABILITY.md    -- ロギング、可観測性、クリーン状態、ベンチマーク
```

新しい機能を追加する場合は、コードを書く前に関連するドキュメントを更新してください。

## Electron レイヤーの境界

### Main Process (`src/main/`)
- `BrowserWindow` のライフサイクルと IPC 登録を担当します。
- ファイルシステムへのアクセスはすべて、ここでサービス経由で行います。
- すべての IPC イベントについて構造化ロギングを行います。

### Preload (`src/preload/`)
- main と renderer をつなぐ唯一のブリッジです。
- `contextBridge.exposeInMainWorld` を使って型付き API を公開します。
- 公開するもの: documents, indexing, qa, feedback, app の各 namespace。

### Renderer (`src/renderer/`)
- React + TypeScript の UI レイヤーです。
- `window.knowledgeBase` API を通してのみ通信します。
- Node.js モジュールは絶対に import しません。

### Services (`src/services/`)
- main process 内の純粋な TypeScript ビジネスロジックです。
- `PersistenceService` はコンストラクタ注入されます。
- すべてのサービスは構造化 JSON 出力のために `logger.forService()` を使います。

## 慣例

- TypeScript は strict mode を使用します。`any` を使う場合は、理由を説明するコメントが必要です。
- export は named export のみです。
- IPC チャンネルは `src/shared/types.ts` で一度だけ定義します。
- 新しい IPC チャンネルは `namespace:action` の形式に従います。
- すべての service メソッドは、重要なイベントで INFO レベルのログを出力しなければなりません。
- 通常のデータアクセスは DEBUG レベルです。
- 不足しているが重大ではないデータは WARN です。
- 失敗は ERROR です。

## 完了条件

機能が「完了」と見なされるのは、次の条件を満たしたときです。

1. TypeScript がエラーなくコンパイルできること (`npm run check`)。
2. アプリが起動し、ウィンドウが表示されること。
3. `feature_list.json` に、その機能が `"pass"` ステータスと証拠付きで記載されること。
4. コードが Electron レイヤーの境界を守っていること。
5. 構造化ロギングがすべての service 操作をカバーしていること。
6. `docs/ARCHITECTURE.md` および/または `docs/PRODUCT.md` が更新されていること。
7. `clean-state-checklist.md` のすべてのチェックに通ること。

## セッション引き継ぎ

作業を再開するときは、前回のセッションの文脈として `session-handoff.md` を読んでください。セッションを終えるときは、次の内容で更新してください。

- 実施した内容
- 残っている内容
- 発生したブロッカーや行った決定
- 変更したファイル
- 該当する場合はベンチマーク結果

## クリーン状態

各主要テストサイクルの前に、次の手順を実行してください。

1. `bash scripts/cleanup-scanner.sh` を実行して、古い成果物がないか確認する。
2. アプリ内の Reset ボタン、または `RESET_DATA` IPC を使ってすべてのデータを消去する。
3. `clean-state-checklist.md` が通ることを確認する。
4. `bash scripts/benchmark.sh` を実行してパフォーマンスを測定する。
