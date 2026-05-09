# AGENTS.md

このリポジトリは、長時間にわたるコーディングエージェント作業を前提にしています。目的は、単純にコード出力量を最大化することではありません。次のセッションが推測なしで続けられる状態でリポジトリを残すことです。

## Startup Workflow

コードを書き始める前に:

1. Confirm the working directory with `pwd`.
2. `claude-progress.md` を読み、最新の検証済み状態と次の手順を確認する。
3. Read `feature_list.json` and choose the highest-priority unfinished feature.
4. `git log --oneline -5` で直近のコミットを確認する。
5. Run `./init.sh`.
6. 新しい作業を始める前に、必要な smoke または end-to-end の検証を実行する。

ベースラインの検証がすでに失敗している場合は、まずそれを修正してください。壊れた開始状態の上に、新しい機能作業を重ねてはいけません。

## Working Rules

- 1つの機能に一度に取り組んでください。
- コードを追加しただけで機能完了にしないでください。
- 選択した機能の範囲内で変更を行い、ブロッカーのために限定的な補助修正が必要な場合を除いて、範囲外へ広げないでください。
- 実装中に検証ルールを黙って変更しないでください。
- チャットの要約よりも、リポジトリ内に残る永続的な成果物を優先してください。

## Runtime Observability

すべてのサービスは `src/services/logger.ts` を通じて構造化ログを出力します。ログ出力は JSON 形式で、タイムスタンプ、レベル、サービス名、メッセージを含みます。ログレベルは `DEBUG`, `INFO`, `WARN`, `ERROR` です。

デバッグ時は、ログで次を確認してください:
- 起動時のサービス初期化イベント
- IPC チャネルの呼び出しとそのパラメータ
- インデックス化されたチャンク数と内容の長さ
- Q&A の信頼度スコアと引用数

## Architecture Constraints

次のレイヤー境界は `scripts/check-architecture.sh` によって強制されます:

- **Renderer** は `fs`、`path`、または他の Node.js コアモジュールを import してはいけません。
- **Services** は Electron IPC や renderer 専用モジュールを import してはいけません。
- **Preload** は `contextBridge` を通じて型付き API だけを公開しなければなりません。

コミット前に `bash scripts/check-architecture.sh` を実行してください。

## Required Artifacts

- `feature_list.json`: 機能状態の唯一の基準
- `claude-progress.md`: セッションログと現在の検証済み状態
- `init.sh`: 標準の起動および検証手順
- `session-handoff.md`: 大きなセッション向けの任意の簡潔な引き継ぎ
- `clean-state-checklist.md`: コミット前のリポジトリ健全性チェック

## Definition Of Done

機能が完了とみなされるのは、次のすべてを満たした場合のみです:

- 目的の動作が実装されている
- 必要な検証が実際に実行された
- 証跡が `feature_list.json` または `claude-progress.md` に記録されている
- 標準の起動手順からリポジトリを再起動できる状態が保たれている
- `scripts/check-architecture.sh` が違反なしで通る

## End Of Session

セッションを終了する前に:

1. Update `claude-progress.md`.
2. Update `feature_list.json`.
3. 未解決のリスクやブロッカーを記録する。
4. `bash scripts/check-architecture.sh` を実行する。
5. 作業が安全な状態になったら、内容が分かるコミットメッセージでコミットする。
6. 次のセッションがすぐに `./init.sh` を実行できる程度に、リポジトリをきれいな状態で残す。
