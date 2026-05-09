[中国語版 →](../../../zh/projects/project-01-baseline-vs-minimal-harness/)

> 関連講義: [Lecture 01. 強力なモデルでも確実に実行できるとは限らない](./../../lectures/lecture-01-why-capable-agents-still-fail/index.md) · [Lecture 02. harness が実際に意味するもの](./../../lectures/lecture-02-what-a-harness-actually-is/index.md)
> Template files: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/en/resources/templates/)

# Project 01. Prompt-Only vs. Rules-First: どのくらい違いが出るのか

## やること

最小構成の Electron ナレッジベースアプリのひな形を作ります。ウィンドウには、左にドキュメント一覧、右に Q&A パネル、そしてローカルのデータディレクトリを持たせます。タスク自体は複雑ではありません。複雑なのは、それをどうやってエージェントに完了させるかです。

これを2回実行します。1回目は、準備なしでプロンプトだけを与えます。2回目は、`AGENTS.md`、`init.sh`、`feature_list.json` をあらかじめリポジトリに配置してから実行します。そのうえで比較します。

このプロジェクトの本質は、コードを書くことではありません。「先に15分かけてルールを整える」のと「そのままエージェントに任せる」の間に、どれだけ差が出るかを確かめることです。

## 使うもの

- Claude Code か Codex（どちらか1つを選び、2回とも同じものを使う）
- Git（ブランチ管理と比較）
- Node.js + Electron（プロジェクトの技術スタック）
- タイマー（各実行時間を記録する）

## ハーネスの仕組み

最小構成のハーネス: `AGENTS.md` + `init.sh` + `feature_list.json`
