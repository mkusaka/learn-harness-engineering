# 初期化エージェントプレイブック (Initializer Agent Playbook)

この playbook は、リポジトリで最初の本格的なセッション、つまり段階的な機能作業が始まる前に使ってください。初期化(initialization) は、その後のすべてのセッションが安定して動作できる土台を整える段階です。

## 目標 (Goal)

その後のセッションが、開始コマンド、現在の状態、作業境界をあらためて導き出さなくても実装を進められるよう、安定した操作基盤(operating surface) を作ることです。

## 必須成果物 (Required Outputs)

初期化エージェント(agent) は、少なくとも次の成果物を残さなければなりません。

- `AGENTS.md` または `CLAUDE.md` のようなルート指示ファイル
- `feature_list.json` のような機械可読な機能一覧
- `claude-progress.md` のような継続的な進捗成果物
- `init.sh` のような標準開始ヘルパー(helper)
- ベースライン(baseline) の scaffold を記録する初期の安全な commit

## チェックリスト (Checklist)

1. 標準の開始手順を定義します。
2. 標準の検証(verification) 手順を定義します。
3. 進捗ログ(progress log) を作成し、開始時点の状態を記録します。
4. 作業を、status を持つ明示的な機能に分解します。
5. 最初のクリーン(clean) なベースライン commit を作成します。

## 成功テスト (Success Test)

以前の chat context がない新しいセッションでも、次の質問に答えられる必要があります。

- このリポジトリが何をするものか
- どうやって開始するか
- どうやって verify するか
- 何が未完成か
- 次に取るべき最善の行動は何か
