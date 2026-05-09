[英語版 →](../../../en/projects/project-01-baseline-vs-minimal-harness/) | [中国語版 →](../../../zh/projects/project-01-baseline-vs-minimal-harness/)

> 関連講義: [第01講. 強いモデルでも確実に実行できるとは限らない](./../../lectures/lecture-01-why-capable-agents-still-fail/index.md) · [第02講. Harness とは実際には何か](./../../lectures/lecture-02-what-a-harness-actually-is/index.md)
> テンプレート: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/vi/resources/templates/)

# プロジェクト01. Prompt だけ vs. ルール優先: どれほど差が出るのか

## 何をするか

最小限の Electron knowledge-base shell アプリを作る。左にドキュメント一覧、右に Q&A パネル、そしてローカルのデータディレクトリを備えた、1 つのウィンドウだけの構成にする。タスク自体は複雑ではない。複雑なのは、それを agent にどう完了させるかだ。

2 回実行する。1 回目は、事前準備なしで単一の prompt だけを与える。2 回目は、`AGENTS.md`、`init.sh`、`feature_list.json` を repo にあらかじめ置いておく。そのうえで比較する。

このプロジェクトの核心は、コードを書くことではない。「先に 15 分かけてルールを整える」ことと、「agent に任せきりにする」ことの差がどれほど大きいのかを確かめることだ。

## ツール

- Claude Code または Codex（どちらか一方を選び、2 回とも同じものを使う）
- Git（ブランチ管理と比較）
- Node.js + Electron（プロジェクトの tech stack）
- ストップウォッチ（各実行時間を記録する）

## Harness の仕組み

最小構成の Harness: `AGENTS.md` + `init.sh` + `feature_list.json`
