# Project 01: Baseline vs Minimal Harness

弱い harness（プロンプトのみ）と、明示的な harness（ルールファイル + 検証機構）が AI コーディングエージェントのタスク完了率に与える影響を比較します。

## ディレクトリ説明

| ディレクトリ | 意味 |
|------|------|
| `starter/` | **出発点**。曖昧な `task-prompt.md` だけがあり、AGENTS.md も feature_list.json もありません。これはエージェントに渡す「弱い harness」版です。 |
| `solution/` | **参考実装**。同じアプリコードですが、完全な harness ファイル（AGENTS.md、feature_list.json、init.sh、claude-progress.md）が揃っています。これは「明示的な harness」版です。 |

## 使い方

```sh
# 1. starter（弱い harness）で一度エージェントタスクを実行する
cd starter
npm install
# task-prompt.md の内容を Claude Code / Codex への prompt として渡す
# エージェントに以下の完了を試させる: ウィンドウ起動、文書一覧、QA パネル、データディレクトリ

# 2. solution（明示的な harness）でもう一度実行する
cd ../solution
npm install
# AGENTS.md を読み、ルールに従って同じタスクを実行させる

# 3. 2 回の結果を比較する
# - タスクは完了したか？
# - 何回再試行が必要だったか？
# - エージェントは途中で「完了した」と先に主張したか？
```

## このプロジェクトで扱う機能

- Electron ウィンドウが正常に起動する
- UI に文書一覧エリアが表示される
- UI に QA パネルが表示される
- アプリがローカルデータディレクトリを作成し、それを使用する

## 対応する講義

- [Lecture 01: なぜ有能なモデルでも失敗するのか](../../docs/lectures/lecture-01-why-capable-agents-still-fail/index.md)
- [Lecture 02: Harness とは実際には何か](../../docs/lectures/lecture-02-what-a-harness-actually-is/index.md)
