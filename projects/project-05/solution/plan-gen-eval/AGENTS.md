# AGENTS.md

このリポジトリは、長時間にわたる coding-agent の作業を前提に設計されています。目的は、単純にコード出力を最大化することではありません。次のセッションが推測なしで続けられる状態でリポジトリを残すことです。

## Startup Workflow

コードを書く前に:

1. `pwd` で作業ディレクトリを確認する。
2. `claude-progress.md` を読み、最新の検証済み状態と次の手順を確認する。
3. `feature_list.json` を読み、最優先の未完了 feature を選ぶ。
4. `git log --oneline -5` で直近のコミットを確認する。
5. `./init.sh` を実行する。
6. 新しい作業を始める前に、必要な smoke test または end-to-end verification を実行する。

ベースラインの verification がすでに失敗している場合は、先にそれを修正すること。壊れた開始状態の上に新しい feature 作業を積み重ねてはいけません。

## Working Rules

- 1つの feature を一度に扱うこと。
- コードを追加しただけで feature 完了と見なしてはいけません。
- ブロッカーがあってどうしても必要な場合を除き、変更は選択した feature の範囲内に収めること。
- 実装中に verification ルールを黙って変更しないこと。
- チャット要約よりも、リポジトリ内に残る永続的な成果物を優先すること。

## Runtime Observability

すべてのサービスは `src/services/logger.ts` 経由で structured logging を使います。ログ出力は JSON 形式で、timestamp、level、service name、message を含みます。ログレベルは `DEBUG`、`INFO`、`WARN`、`ERROR` です。

デバッグ時は、ログで次を確認してください:
- 起動時の service 初期化イベント
- IPC channel の呼び出しとそのパラメータ
- indexing の chunk 数とコンテンツ長
- Q&A の confidence score と citation 数

## Architecture Constraints

次の layer boundary は `scripts/check-architecture.sh` によって強制されます:

- **Renderer** は `fs`、`path`、および任意の Node.js core modules を import してはいけません。
- **Services** は Electron IPC または renderer 固有の modules を import してはいけません。
- **Preload** は contextBridge 経由で typed API だけを公開しなければなりません。

コミット前に `bash scripts/check-architecture.sh` を実行してください。

## Required Artifacts

- `feature_list.json`: feature 状態の source of truth
- `claude-progress.md`: セッションログと現在の検証済み状態
- `init.sh`: 標準の startup と verification の手順
- `session-handoff.md`: 大きなセッション向けの任意の簡潔な引き継ぎ
- `clean-state-checklist.md`: コミット前のリポジトリ健全性チェック

## Definition Of Done

feature が完了したと言えるのは、次のすべてを満たした場合だけです:

- 対象の behavior が実装されている
- 必要な verification が実際に実行された
- その証拠が `feature_list.json` または `claude-progress.md` に記録されている
- 標準の startup path から再起動可能な状態が保たれている
- `scripts/check-architecture.sh` が違反なしで通過する

## End Of Session

セッションを終える前に:

1. `claude-progress.md` を更新する。
2. `feature_list.json` を更新する。
3. 未解決の risk または blocker を記録する。
4. `bash scripts/check-architecture.sh` を実行する。
5. 作業が安全な状態になったら、説明的な message で commit する。
6. 次のセッションがすぐに `./init.sh` を実行できるだけの clean な状態でリポジトリを残す。
