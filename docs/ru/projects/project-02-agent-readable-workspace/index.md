[中国語版 →](../../../zh/projects/project-02-agent-readable-workspace/)

> 関連講義: [講義 03. リポジトリを唯一の真実の源にする](./../../lectures/lecture-03-why-the-repository-must-become-the-system-of-record/index.md) · [講義 04. 指示をファイルごとに分割する](./../../lectures/lecture-04-why-one-giant-instruction-file-fails/index.md)
> テンプレートファイル: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/en/resources/templates/)

# プロジェクト 02. プロジェクトを読みやすくして、中断したところから再開する

## 何をするか

リポジトリに「読みやすさ」を備えさせ、新しいエージェントがプロジェクト構造をすばやく理解し、現在の進捗を把握して、そのまま作業を引き継げるようにします。具体的には、ドキュメントの取り込み、ドキュメント詳細の表示、ローカル保存を実装し、それを 2 回のセッションで完了させます。

この課題は 2 回実行します。1 回目は何の補助もない状態で、2 回目はあらかじめリポジトリ内に `ARCHITECTURE.md`、`PRODUCT.md`、`session-handoff.md` が置かれた状態で実行します。

## ツール

- Claude Code または Codex
- Git
- Node.js + Electron

## harness の仕組み

エージェントが読み取れるワークスペースと、永続化された状態ファイル
