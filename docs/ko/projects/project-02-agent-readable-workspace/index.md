[英語版 →](../../../en/projects/project-02-agent-readable-workspace/)

> 関連講義: [講義 03. リポジトリを単一の真実の源にする](./../../lectures/lecture-03-why-the-repository-must-become-the-system-of-record/index.md) · [講義 04. 指示を複数のファイルに分割する](./../../lectures/lecture-04-why-one-giant-instruction-file-fails/index.md)
> テンプレートファイル: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/en/resources/templates/)

# プロジェクト 02. プロジェクトを読みやすくし、中断したところから再開できるようにする

## やること

新しいエージェント(agent)がプロジェクト構造をすばやく把握し、現在の進捗を理解し、そのまま作業を引き継げるように、リポジトリに「読みやすさ(readability)」を追加します。具体的には、ドキュメントの import、ドキュメントの詳細表示、ローカルな永続化(persistence)を実装し、2つのセッションを通して完成させます。

2回実行します。1回目は何の助けもない状態で、2回目は `ARCHITECTURE.md`、`PRODUCT.md`、`session-handoff.md` をリポジトリにあらかじめ配置した状態で進めます。

エージェントは、コンテキストウィンドウ(context window)が初期化されると、以前の作業の流れを覚えていません。session handoff ファイルに前回セッションの状態(state)を永続化しておけば、新しいセッションのエージェントも同じ進行地点から始められます。

## ツール

- Claude Code または Codex
- Git
- Node.js + Electron

## ハーネスの仕組み

エージェントが読みやすい作業空間 + 永続状態ファイル(persistent state files)
