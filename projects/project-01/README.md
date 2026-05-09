# Project 01: ベースラインと最小ハーネス

弱いハーネス（プロンプトのみ）と、明示的なハーネス（ルールファイルと検証機構を含む）が、AI コーディングエージェントのタスク完了率にどう影響するかを比較します。

## ディレクトリガイド

| Directory | 意味 |
|------|------|
| `starter/` | **開始点**: 曖昧な `task-prompt.md` だけがあり、AGENTS.md も feature_list.json もありません。これはエージェントに渡す「弱いハーネス」版です。 |
| `solution/` | **参考実装**: 同じアプリケーションコードですが、完全なハーネスファイル（AGENTS.md、feature_list.json、init.sh、claude-progress.md）が含まれています。これが「明示的なハーネス」版です。 |

## 使い方

```sh
# 1. starter（弱いハーネス）でエージェントのタスクを1回実行する
cd starter
npm install
# task-prompt.md の内容を Claude Code / Codex へのプロンプトとして渡す
# エージェントに、ウィンドウの起動、ドキュメント一覧、QA パネル、データディレクトリの実装を依頼する

# 2. solution（明示的なハーネス）でも同じタスクを実行する
cd ../solution
npm install
# エージェントに AGENTS.md を読ませ、同じタスクでルールに従うよう依頼する

# 3. 2つの結果を比較する
# - タスクは完了したか？
# - 何回の再試行が必要だったか？
# - エージェントは早すぎる段階で「完了」と主張しなかったか？
```

## 対象機能

- Electron ウィンドウが正常に起動する
- UI にドキュメント一覧エリアが表示される
- UI に QA パネルが表示される
- アプリがローカルのデータディレクトリを作成して使用する

## 関連講義

- [Lecture 01: Why Capable Agents Still Fail](../../docs/en/lectures/lecture-01-why-capable-agents-still-fail/index.md)
- [Lecture 02: What a Harness Actually Is](../../docs/en/lectures/lecture-02-what-a-harness-actually-is/index.md)
