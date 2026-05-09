[英語版](./README.md) · [中国語版](./README-CN.md) · **韓国語版**

# スキル(Skills)

このディレクトリには、Learn Harness Engineering プロジェクト向けの再利用可能な AI エージェント(agent)スキル(skill)が入っています。各スキルは、AI コーディングエージェント(Claude Code, Codex, Cursor, Windsurf など)が読み込み、特定の作業を実行できる独立したプロンプト(prompt)テンプレートです。

スキルは、エージェントが複雑な作業フローをモジュール化された形で処理できるようにするハーネス(harness)の中核要素です。`SKILL.md` ファイルをエントリポイントにする構造化された形式に従います。

## 利用可能なスキル

### harness-creator

AI コーディングエージェント向けのプロダクションハーネスエンジニアリングスキルです。エージェントハーネスファイル(AGENTS.md、機能一覧、検証ワークフロー、セッション継続性(session continuity)メカニズム)の作成、評価、改善を支援します。

- **5つの参照パターン**: メモリ永続化(Memory Persistence)、コンテキストエンジニアリング(Context Engineering)、ツールレジストリ(Tool Registry)、マルチエージェント調整(Multi-Agent Coordination)、ライフサイクルおよびブートストラップ(Lifecycle & Bootstrap)
- **テンプレート**: AGENTS.md, feature-list.json, init.sh, progress.md
- **5つの組み込み評価(eval)テストケース**
- **バイリンガル**: 英語 + 中文

詳細は [harness-creator/README.md](harness-creator/README.md) を参照してください。

## harness-creator の構築方法

`harness-creator` スキルは **skill-creator** 方法論を使って開発されました。skill-creator は、エージェントスキルを作成・テスト・反復するための Anthropic の公式メタスキルです。skill-creator は、組み込みの評価実行器、採点器、ベンチマークビューアを備えた構造化ワークフロー(ドラフト → テスト → 評価 → 反復)を提供します。

このように、スキル自体をスキルとして扱うアプローチは、ハーネスエンジニアリングの再帰的な性質を示しています。エージェント向けのツールを、エージェントが検証する構造です。

- **skill-creator ソース**: [anthropics/skills — skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator)
- **Anthropic Claude Code スキル文書**: [anthropics/claude-code — plugin-dev/skills](https://github.com/anthropics/claude-code/tree/main/plugins/plugin-dev/skills)

## ディレクトリ構成

```
skills/
├── README.md                    # 英語版 (このファイルの原本)
├── README-CN.md                 # 中国語版
├── README-KO.md                 # 韓国語版 (このファイル)
└── harness-creator/             # ハーネスエンジニアリングスキル
    ├── SKILL.md                 # 主要なスキル定義 (バイリンガル)
    ├── SKILL.md.en              # 英語のみの版
    ├── README.md                # 詳細文書
    ├── metadata.json            # スキルメタデータとトリガー
    ├── evals/                   # テストケース
    ├── templates/               # スキャフォールド(scaffold)テンプレート
    └── references/              # 詳細なパターン文書
```

## スキルの動作方式

各スキルは標準構造に従います。

1. **SKILL.md** — エントリポイントです。YAML フロントマター(frontmatter)(名前、トリガー用の説明)と、エージェント向けの Markdown 指示が含まれています。
2. **references/** — 必要に応じてコンテキストとして読み込まれる追加文書です。
3. **templates/** — スキルがユーザー向けに生成できる開始テンプレートです。

スキルは段階的開示(progressive disclosure)を使用します。エージェントは最初に名前と説明だけを見て、トリガーされると `SKILL.md` の全文を読み込み、必要なときだけ同梱リソースを読みます。

このアプローチは、コンテキストウィンドウ(context window)を効率的に使うためのものです。エージェントは常にすべての情報を持った状態で始めるのではなく、必要なタイミングで必要な情報を読み込みます。

## セキュリティ監査

このディレクトリ内のすべてのファイルはセキュリティ監査済みです。

- バックドア、隠し URL、エンコードされたペイロードはありません
- データ漏えいまたはハードコードされた認証情報はありません
- コマンドインジェクション脆弱性はありません
- `init.sh` は標準の npm ライフサイクルコマンドのみを実行します

## ライセンス

MIT
