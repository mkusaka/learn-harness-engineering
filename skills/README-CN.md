# スキル（技能集）

[英語版](./README.md) · [韓国語版](./README-KO.md)

このディレクトリには、Learn Harness Engineering プロジェクトで再利用できる AI エージェント向けスキルが含まれています。各スキルは自己完結型のプロンプトテンプレートであり、AI コーディングエージェント（Claude Code、Codex、Cursor、Windsurf など）が読み込んで、専門的なタスクを実行できます。

## 利用可能なスキル

### harness-creator

AI コーディングエージェント向けの本番品質の harness エンジニアリングスキルです。agent harness ファイル（AGENTS.md、機能一覧、検証ワークフロー、会話継続メカニズム）の作成、評価、改善を支援します。

- **5 つの参考パターン**：記憶の永続化、コンテキストエンジニアリング、ツール登録、マルチエージェント協調、ライフサイクルと起動
- **テンプレート**：AGENTS.md、feature-list.json、init.sh、progress.md
- **5 つの組み込み評価テストケース**
- **バイリンガル対応**：English + 中国語

詳細は [harness-creator/README.md](harness-creator/README.md) を参照してください。

## harness-creator の開発プロセス

`harness-creator` スキルは **skill-creator** の方法論を基に開発されています。これは Anthropic が公式に提供しているメタスキルで、agent スキルの作成、テスト、反復改善に使います。skill-creator には、下書き → テスト → 評価 → 反復 という構造化されたワークフローと、組み込みの評価ランナー、スコアラー、ベンチマークビューアが備わっています。

- **skill-creator の出典**：[anthropics/skills — skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator)
- **Anthropic Claude Code のスキル文書**：[anthropics/claude-code — plugin-dev/skills](https://github.com/anthropics/claude-code/tree/main/plugins/plugin-dev/skills)

## ディレクトリ構成

```
skills/
├── README.md                    # 英語版の説明
├── README-CN.md                 # 中国語版の説明（本ファイル）
└── harness-creator/             # Harness エンジニアリングスキル
    ├── SKILL.md                 # メインのスキル定義（バイリンガル）
    ├── SKILL.md.en              # 英語版
    ├── README.md                # 詳細ドキュメント
    ├── metadata.json            # スキルのメタデータとトリガーワード
    ├── evals/                   # テストケース
    ├── templates/               # スキャフォールド用テンプレート
    └── references/              # 詳細なパターン参照ドキュメント
```

## スキルの動作方式

各スキルは標準構成に従います。

1. **SKILL.md** — エントリーファイルです。YAML frontmatter（name、description、トリガー用）と Markdown の指示を含みます。
2. **references/** — 必要に応じてコンテキストに読み込まれる補足ドキュメントです。
3. **templates/** — スキルがユーザー向けに生成する開始テンプレートです。

スキルは段階的な展開を採用しています。agent は最初に名前と説明だけを見て、トリガー後に完全な `SKILL.md` 本文を読み込み、付随するリソースファイルは必要なときだけ参照します。

## セキュリティ監査

このディレクトリ内のすべてのファイルはセキュリティ監査を通過しています。

- バックドア、隠し URL、エンコードされたペイロードはありません
- データ漏えい、ハードコードされた認証情報はありません
- コマンドインジェクションの脆弱性はありません
- `init.sh` は標準の npm ライフサイクルコマンドのみを実行します

## ライセンス

MIT
