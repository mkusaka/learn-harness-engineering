# AGENTS.md

このリポジトリは、長時間にわたるコーディングエージェント作業向けに設計されています。目的は、単純なコード出力量を最大化することではありません。次のセッションが推測なしで作業を継続できる状態でリポジトリを残すことです。

## Startup Workflow

コードを書く前に:

1. `pwd` で作業ディレクトリを確認する。
2. `claude-progress.md` を読み、最新の確認済み状態と次の手順を把握する。
3. `feature_list.json` を読み、最優先の未完了機能を選ぶ。
4. `git log --oneline -5` で直近のコミットを確認する。
5. `./init.sh` を実行する。
6. 新しい作業を始める前に、必要な smoke 検証または E2E 検証を実行する。

ベースライン検証がすでに失敗している場合は、まずそれを修正してください。壊れた開始状態の上に新しい機能作業を積み重ねてはいけません。

## Working Rules

- 1つの機能を一度に1つずつ扱うこと。
- コードを追加しただけで機能完了と見なしてはいけません。
- 選択した機能の範囲内に変更を収めること。ブロッカーにより、限定的な補助修正が必要な場合を除きます。
- 実装中に検証ルールを黙って変更してはいけません。
- チャットの要約よりも、リポジトリ内に残る永続的な成果物を優先してください。

## Runtime Observability

すべてのサービスは `src/services/logger.ts` を通じて構造化ログを使用します。ログ出力は、タイムスタンプ、レベル、サービス名、メッセージを含む JSON 形式です。ログレベルは DEBUG, INFO, WARN, ERROR です。

デバッグ時は、ログで次の項目を確認してください:
- 起動時のサービス初期化イベント
- IPC チャンネルの呼び出しとその引数
- インデックス化されたチャンク数とコンテンツ長
- Q&A の信頼度スコアと引用数

## Architecture Constraints

次のレイヤー境界は `scripts/check-architecture.sh` によって強制されます:

- **Renderer** は `fs`、`path`、その他の Node.js コアモジュールを import してはなりません。
- **Services** は Electron IPC や renderer 固有のモジュールを import してはなりません。
- **Preload** は contextBridge 経由で型付き API のみを公開しなければなりません。

コミット前に `bash scripts/check-architecture.sh` を実行してください。

## Required Artifacts

- `feature_list.json`: 機能状態の唯一の正本
- `claude-progress.md`: セッションログと現在の確認済み状態
- `init.sh`: 標準の起動および検証手順
- `session-handoff.md`: 大きなセッション向けの任意の簡潔な引き継ぎ
- `clean-state-checklist.md`: コミット前のリポジトリ健全性チェック

## Definition Of Done

機能が完了したと言えるのは、次のすべてが満たされたときだけです:

- 対象の振る舞いが実装されている
- 必要な検証が実際に実行された
- 証拠が `feature_list.json` または `claude-progress.md` に記録されている
- リポジトリが標準の起動手順から再開可能なままである
- `scripts/check-architecture.sh` が違反なしで通る

## End Of Session

セッションを終了する前に:

1. `claude-progress.md` を更新する。
2. `feature_list.json` を更新する。
3. 未解決のリスクやブロッカーを記録する。
4. `bash scripts/check-architecture.sh` を実行する。
5. 作業が安全な状態になったら、説明的なメッセージでコミットする。
6. 次のセッションがすぐに `./init.sh` を実行できる程度に、リポジトリをクリーンな状態で残す。
