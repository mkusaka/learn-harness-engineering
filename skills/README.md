# スキル

[中国語版](./README-CN.md) · [韓国語版](./README-KO.md)

このディレクトリには、Learn Harness Engineering プロジェクト向けの再利用可能な AI エージェント用スキルが含まれています。各スキルは独立したプロンプトテンプレートであり、AI コーディングエージェント（Claude Code、Codex、Cursor、Windsurf など）が読み込んで、専門的なタスクを実行できます。

## 利用可能なスキル

### harness-creator

AI コーディングエージェント向けの本番用ハーネスエンジニアリングスキルです。エージェント用ハーネスファイル（AGENTS.md、機能一覧、検証ワークフロー、セッション継続メカニズム）の作成、評価、改善を支援します。

- **5 つの参照パターン**: Memory Persistence, Context Engineering, Tool Registry, Multi-Agent Coordination, Lifecycle & Bootstrap
- **テンプレート**: AGENTS.md, feature-list.json, init.sh, progress.md
- **5 つの組み込み評価テストケース**
- **バイリンガル**: 英語 + 中国語

詳細は [harness-creator/README.md](harness-creator/README.md) を参照してください。

## harness-creator の作成方法

`harness-creator` スキルは、エージェント用スキルの作成、テスト、反復改善のための Anthropic 公式メタスキルである **skill-creator** の手法を使って開発されました。skill-creator は、組み込みの評価実行環境、採点機能、ベンチマークビューアを備えた、構造化されたワークフロー（下書き → テスト → 評価 → 反復）を提供します。

- **skill-creator のソース**: [anthropics/skills — skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator)
- **Anthropic Claude Code の skills ドキュメント**: [anthropics/claude-code — plugin-dev/skills](https://github.com/anthropic/claude-code/tree/main/plugins/plugin-dev/skills)

## ディレクトリ構成

```
skills/
├── README.md                    # このファイル
├── README-CN.md                 # 中国語版
└── harness-creator/             # ハーネスエンジニアリングスキル
    ├── SKILL.md                 # メインのスキル定義（バイリンガル）
    ├── SKILL.md.en              # 英語版のみ
    ├── README.md                # 詳細ドキュメント
    ├── metadata.json            # スキルのメタデータとトリガー
    ├── evals/                   # テストケース
    ├── templates/               # スキャフォールド用テンプレート
    └── references/              # パターンの詳細解説ドキュメント
```

## スキルの仕組み

各スキルは、次の標準構成に従います。

1. **SKILL.md** — エントリーポイントです。YAML frontmatter（name、トリガー用の description）と、エージェント向けの Markdown 指示が含まれます。
2. **references/** — 必要に応じてコンテキストに読み込まれる追加ドキュメントです。
3. **templates/** — スキルがユーザー向けに生成できる、起点となるテンプレートです。

スキルは段階的開示を採用しています。エージェントは最初に name と description だけを見て、トリガーされた時点で完全な SKILL.md 本文を読み込み、同梱リソースは必要なときだけ参照します。

## セキュリティ監査

このディレクトリ内のすべてのファイルは、セキュリティ監査済みです。

- バックドア、隠し URL、エンコードされたペイロードはありません
- データの持ち出しやハードコードされた認証情報はありません
- コマンドインジェクションの脆弱性はありません
- `init.sh` は標準の npm ライフサイクルコマンドのみを実行します

## ライセンス

MIT
