[英語版 →](../../../en/projects/project-04-incremental-indexing/) | [中国語版 →](../../../zh/projects/project-04-incremental-indexing/)

> 関連講義: [講義 07. agent のタスク境界を明確にする](./../../lectures/lecture-07-why-agents-overreach-and-under-finish/index.md) · [講義 08. feature list で agent の行動を制約する](./../../lectures/lecture-08-why-feature-lists-are-harness-primitives/index.md)
> テンプレート: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/vi/resources/templates/)

# プロジェクト 04. Runtime フィードバックで agent の挙動を調整する

## やること

起動ログ、import/indexing ログ、エラー状態などの runtime の可観測性と、レイヤーをまたぐ逸脱を防ぐためのアーキテクチャ制約を追加します。エージェントが修正できる runtime バグも仕込みます。

2 回実行します。1 回目はログも制約もなし、2 回目は適切なツールとルールを導入した状態で実行します。

## ツール

- Claude Code または Codex
- Git
- Node.js + Electron

## Harness の考え方

runtime フィードバック + スコープ制御 + 段階的 indexing
