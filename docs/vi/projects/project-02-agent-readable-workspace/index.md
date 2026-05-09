[英語版 →](../../../en/projects/project-02-agent-readable-workspace/) | [中国語版 →](../../../zh/projects/project-02-agent-readable-workspace/)

> 関連講義: [第03講. リポジトリを唯一の正しい情報源にする](./../../lectures/lecture-03-why-the-repository-must-become-the-system-of-record/index.md) · [第04講. 指示を複数のファイルに分割する](./../../lectures/lecture-04-why-one-giant-instruction-file-fails/index.md)
> サンプルテンプレート: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/vi/resources/templates/)

# プロジェクト 02. プロジェクトを読み取れるようにし、中断地点から再開できるようにする

## 何をするか

repo に「読みやすさ」を加えて、新しい agent がプロジェクト構造をすばやく理解し、現在の進捗を把握し、そのまま作業を続けられるようにします。具体的には、ドキュメントの import、document detail view、local persistence を実装し、2回のセッションで完了させます。

2回実行します。1回目は何の支援もなし、2回目は `ARCHITECTURE.md`、`PRODUCT.md`、`session-handoff.md` が repo にあらかじめ置かれた状態で行います。

## 使用ツール

- Claude Code または Codex
- Git
- Node.js + Electron

## ハーネスの仕組み

エージェントが読み取れるワークスペースと、継続的に保持される状態ファイル
