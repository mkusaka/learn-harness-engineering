[中国語版 →](../../../zh/projects/project-01-baseline-vs-minimal-harness/)

> 関連講義: [Lecture 01. 強力なモデルでも確実に実行できるとは限らない](./../../lectures/lecture-01-why-capable-agents-still-fail/index.md) · [Lecture 02. harness とは実際には何か](./../../lectures/lecture-02-what-a-harness-actually-is/index.md)
> テンプレートファイル: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/en/resources/templates/)

# プロジェクト 01. プロンプトだけ vs. ルール優先: どれほど差が出るのか

## やること

ナレッジベース向けの最小限の Electron アプリの土台を作ります。左にドキュメント一覧、右に Q&A パネル、さらにローカルのデータディレクトリを備えた構成です。課題そのものは難しくありません。難しいのは、エージェントに最後までやり切らせることです。

これを 2 回実行します。1 回目は、事前準備なしでプロンプトだけを与えます。2 回目は、`AGENTS.md`、`init.sh`、`feature_list.json` をあらかじめリポジトリに置いた状態で実行します。最後に、その結果を比較します。

このプロジェクトの本質は、コードを書くことではありません。「ルール整備に 15 分かける」ことと「ただエージェントを放り出す」ことの差が、どれほど大きいかを理解することです。

## ツール

- Claude Code または Codex（どちらか 1 つを選び、両方の実行で使う）
- Git（ブランチ管理と比較）
- Node.js + Electron（プロジェクトの技術スタック）
- タイマー（各実行時間を記録する）

## harness の仕組み

最小限の harness: `AGENTS.md` + `init.sh` + `feature_list.json`
