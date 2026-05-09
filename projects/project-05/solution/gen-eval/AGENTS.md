# AGENTS.md

このリポジトリは、長時間にわたるコーディングエージェント作業を前提に設計されています。目的は、単純にコード出力量を最大化することではありません。次のセッションが推測なしで続けられる状態でリポジトリを残すことです。

## Startup Workflow

コードを書く前に:

1. Confirm the working directory with `pwd`.
2. `claude-progress.md` を読み、最新の確認済み状態と次の手順を把握する。
3. `feature_list.json` を読み、優先度が最も高い未完了の機能を選ぶ。
4. `git log --oneline -5` で直近のコミットを確認する。
5. Run `./init.sh`.
6. 新しい作業を始める前に、必要なスモークテストまたは E2E 検証を実行する。

ベースラインの検証がすでに失敗している場合は、まずそれを修正してください。壊れた開始状態の上に新しい機能作業を重ねないでください。

## Working Rules

- 1つの機能を一度に1件ずつ進めること。
- コードを追加しただけで機能完了とみなさないこと。
- ブロッカーによって限定的な補助修正が必要になる場合を除き、変更は選択した機能の範囲内に収めること。
- 実装中に検証ルールを黙って変更しないこと。
- チャットの要約よりも、リポジトリ内に残る永続的な成果物を優先すること。

## Runtime Observability

すべてのサービスは `src/services/logger.ts` を通じて構造化ログを出力します。ログ出力は JSON 形式で、タイムスタンプ、レベル、サービス名、メッセージを含みます。ログレベルは `DEBUG`, `INFO`, `WARN`, `ERROR` です。

デバッグ時は、次の内容がログに出ているか確認してください:
- 起動時のサービス初期化イベント
- IPC チャンネルの呼び出しとその引数
- インデックス作成時のチャンク数とコンテンツ長
- Q&A の信頼度スコアと引用数

## Architecture Constraints

次のレイヤー境界は `scripts/check-architecture.sh` によって強制されます:

- **Renderer** は `fs`、`path`、または任意の Node.js コアモジュールを import してはなりません。
- **Services** は Electron IPC や renderer 固有のモジュールを import してはなりません。
- **Preload** は、`contextBridge` 経由で型付き API だけを公開しなければなりません。

コミット前に `bash scripts/check-architecture.sh` を実行してください。

## Required Artifacts

- `feature_list.json`: 機能状態の正本
- `claude-progress.md`: セッションログと現在の確認済み状態
- `init.sh`: 標準的な起動および検証手順
- `session-handoff.md`: 大きなセッション向けの任意の簡潔な引き継ぎ
- `clean-state-checklist.md`: コミット前のリポジトリ健全性チェック

## Definition Of Done

機能が完了といえるのは、次のすべてを満たした場合だけです:

- 目的の動作が実装されている
- 必要な検証が実際に実行されている
- 証跡が `feature_list.json` または `claude-progress.md` に記録されている
- 標準の起動手順からリポジトリを再起動できる状態が保たれている
- `scripts/check-architecture.sh` が違反なしで通る

## End Of Session

セッションを終える前に:

1. Update `claude-progress.md`.
2. Update `feature_list.json`.
3. 未解決のリスクやブロッカーを記録する。
4. `bash scripts/check-architecture.sh` を実行する。
5. 作業が安全な状態になったら、説明的なメッセージでコミットする。
6. 次のセッションがすぐに `./init.sh` を実行できる程度に、リポジトリをきれいな状態で残す。
