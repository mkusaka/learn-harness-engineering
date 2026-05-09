# English Resource Library

このフォルダは、コースの手法を、そのまま使えるテンプレートと簡潔な
リファレンスに落とし込み、実際のリポジトリで使えるようにします。

## When To Use It

Codex、Claude Code、または他のコーディングエージェントに、セットアップ・進捗・
スコープを毎回ゼロから再構成させずに、複数セッションにまたがって作業させたいときは
ここから始めてください。

特に次のような場合に有効です。

- 作業が複数セッションにまたがる
- 機能が多く、未完了のまま残りやすい
- エージェントが早々に完了宣言しがち
- 起動手順を毎回再発見してしまう

## Start Here

最小構成なら、まず以下を用意してください。

- root instructions: [`templates/AGENTS.md`](./templates/AGENTS.md) or [`templates/CLAUDE.md`](./templates/CLAUDE.md)
- feature state: [`templates/feature_list.json`](./templates/feature_list.json)
- progress log: [`templates/claude-progress.md`](./templates/claude-progress.md)
- bootstrap script reference: `docs/en/resources/templates/init.sh`

次に、以下を追加します。

- session handoff: [`templates/session-handoff.md`](./templates/session-handoff.md)
- clean-exit checklist: [`templates/clean-state-checklist.md`](./templates/clean-state-checklist.md)
- evaluator rubric: [`templates/evaluator-rubric.md`](./templates/evaluator-rubric.md)

「Harness engineering」記事で紹介している、より充実した OpenAI 風の
リポジトリ構成が欲しい場合は、代わりに advanced pack を使ってください。

- [`openai-advanced/index.md`](./openai-advanced/index.md)

## Library Structure

- [`templates/`](./templates/index.md): 実際の repo にコピーするためのテンプレート
- [`reference/`](./reference/index.md): 手法メモ、起動フロー、障害モードのマップ
- [`openai-advanced/`](./openai-advanced/index.md): advanced な repo の骨格、
  system-of-record 用ドキュメント、agent-first のガバナンステンプレート

## Recommended Minimal Pack

- `AGENTS.md` or `CLAUDE.md`
- `feature_list.json`
- `claude-progress.md`
- `init.sh`

この 4 ファイルがあれば、ほとんどのエージェントワークフローは目に見えて安定します。

リポジトリが、複数ドメイン・進行中の計画・品質スコアリング・信頼性ポリシーを持つ
長期運用システムに成長したら、最小構成を無理に引き延ばすのではなく、
[`openai-advanced/`](./openai-advanced/index.md) パックへ移行してください。
