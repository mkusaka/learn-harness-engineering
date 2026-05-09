# AGENTS.md

このリポジトリは、長時間にわたる coding-agent 作業を前提に設計されています。目的は、単純なコード出力量を最大化することではありません。次のセッションが推測なしで作業を続けられる状態にリポジトリを保つことです。

## Startup Workflow

コードを書き始める前に:

1. `pwd` で作業ディレクトリを確認する。
2. `claude-progress.md` を読み、最新の検証済み状態と次のステップを確認する。
3. `feature_list.json` を読み、優先度が最も高い未完了の feature を選ぶ。
4. `git log --oneline -5` で直近のコミットを確認する。
5. `./init.sh` を実行する。
6. 新しい作業を始める前に、必要な smoke または end-to-end の検証を実行する。

ベースライン検証がすでに失敗している場合は、まずそれを修正してください。壊れた開始状態の上に新しい feature 作業を積み重ねてはいけません。

## Working Rules

- 一度に扱う feature は 1 つだけにする。
- コードを追加しただけで feature 完了とみなさない。
- blocker によって限定的な補助修正が必要になる場合を除き、変更は選択した feature の範囲内に収める。
- 実装の途中で検証ルールを黙って変えない。
- チャットの要約よりも、リポジトリに残る永続的な成果物を優先する。

## Runtime Observability

すべての service は `src/services/logger.ts` を通じて structured logging を使用します。ログ出力は、timestamp、level、service name、message を含む JSON 形式です。ログレベルは `DEBUG`, `INFO`, `WARN`, `ERROR` です。

デバッグ時は、以下のログを確認してください:
- 起動時の service 初期化イベント
- IPC channel の呼び出しとその parameters
- indexing の chunk 数と content の長さ
- Q&A の confidence score と citation 数

## Architecture Constraints

次の layer boundary は `scripts/check-architecture.sh` によって強制されています:

- **Renderer** は `fs`、`path`、その他の Node.js core module を import してはいけません。
- **Services** は Electron IPC や renderer 専用 module を import してはいけません。
- **Preload** は `contextBridge` 経由で typed API のみを公開しなければなりません。

commit 前に `bash scripts/check-architecture.sh` を実行してください。

## Required Artifacts

- `feature_list.json`: feature 状態の single source of truth
- `claude-progress.md`: session log と現在の検証済み状態
- `init.sh`: 標準の起動・検証手順
- `session-handoff.md`: 大きな session 向けの任意の簡潔な引き継ぎ
- `clean-state-checklist.md`: commit 前の repository health check

## Definition Of Done

feature が done と言えるのは、次のすべてを満たした場合だけです:

- 対象の behavior が実装されている
- 必要な検証が実際に実行された
- 証拠が `feature_list.json` または `claude-progress.md` に記録されている
- リポジトリが標準の起動手順から再起動可能な状態を保っている
- `scripts/check-architecture.sh` が違反なしで通る

## End Of Session

セッションを終える前に:

1. `claude-progress.md` を更新する。
2. `feature_list.json` を更新する。
3. 未解決の risk や blocker を記録する。
4. `bash scripts/check-architecture.sh` を実行する。
5. 作業が安全な状態になったら、内容が分かる commit message で commit する。
6. 次のセッションがすぐに `./init.sh` を実行できる程度にリポジトリを clean に保つ。
