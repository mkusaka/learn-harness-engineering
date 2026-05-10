[英語版 →](../../../en/projects/project-04-incremental-indexing/) | [中国語版 →](../../../zh/projects/project-04-incremental-indexing/)

> 関連講義: [Lecture 07. Draw clear task boundaries for agents](./../../lectures/lecture-07-why-agents-overreach-and-under-finish/index.md) · [Lecture 08. Use feature lists to constrain what the agent does](./../../lectures/lecture-08-why-feature-lists-are-harness-primitives/index.md)
> テンプレートファイル: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/ja/resources/templates/)

# Project 04. Runtime Feedback を使って Agent の挙動を修正する

## 取り組む内容

起動ログ、import/indexing ログ、エラー状態など、runtime の可観測性を追加し、レイヤーをまたぐ違反を防ぐためのアーキテクチャ上の制約も加えます。さらに、Agent に修正させる runtime バグも仕込みます。

これを 2 回実行します。1 回目はログも制約もない状態で、2 回目は適切なツールとルールを入れた状態で行います。

## 使用ツール

- Claude Code or Codex
- Git
- Node.js + Electron

## Harness の仕組み

runtime feedback + scope control + incremental indexing
