[中国語版 →](../../../zh/projects/project-02-agent-readable-workspace/)

> 関連講義: [Lecture 03. Make the repository your single source of truth](./../../lectures/lecture-03-why-the-repository-must-become-the-system-of-record/index.md) · [Lecture 04. Split instructions across files](./../../lectures/lecture-04-why-one-giant-instruction-file-fails/index.md)
> テンプレートファイル: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/en/resources/templates/)

# Project 02. プロジェクトを読みやすくし、中断したところから再開できるようにする

## やること

新しい agent がプロジェクト構成をすばやく理解し、現在の進捗を把握して、そのまま作業を引き継げるように、repo に「読みやすさ」を加えます。具体的には、document import、document detail view、local persistence を実装し、2 回のセッションに分けて完了させます。

これを 2 回実行します。1 回目は何の補助もなしで、2 回目は `ARCHITECTURE.md`、`PRODUCT.md`、`session-handoff.md` を repo にあらかじめ配置した状態で行います。

## ツール

- Claude Code or Codex
- Git
- Node.js + Electron

## Harness の仕組み

agent-readable workspace + persistent state files
