# harness-creator スキル

Learn Harness Engineering の講義内容と業界のベストプラクティスを凝縮した、AI コーディングエージェント向けの実運用ハーネス設計スキルです。

## インストール

```bash
npx skills add github:harness-creator
```

または `skills/harness-creator/` ディレクトリを手動で skill パスにコピーします。

## このスキルでできること

このスキルは次のことを支援します。
- **ゼロからハーネスを作る** — AGENTS.md、機能一覧、検証ワークフロー
- **既存のハーネスを改善する** — 5 サブシステム評価と優先度付き改善
- **セッション継続性を設計する** — メモリ永続化、進捗追跡、引き継ぎ手順
- **効果をベンチマークする** — 定量指標による改善前後比較
- **実運用パターンを適用する** — メモリ、コンテキストエンジニアリング、ツール安全性、マルチエージェント協調

## 基本フレームワーク: 5 つのサブシステム

すべてのハーネスは 5 つのサブシステムで構成されます。

1. **Instructions** — ルーティング層としての AGENTS.md、`docs/` 階層による段階的開示
2. **State** — `feature_list.json`、`progress.md`、セッション引き継ぎファイル
3. **Verification** — エージェントが完了を宣言する前に必ず実行しなければならない明示的なコマンド
4. **Scope** — 1 機能ずつ進める方針、明確な完了条件
5. **Lifecycle** — `init.sh`、クリーン状態チェックリスト、セッション継続メカニズム

## 参考パターン

このスキルには、6 つの詳細な参考文書が含まれます。

| パターン | 使う場面 |
|---------|-------------|
| [Memory Persistence](references/memory-persistence-pattern.md) | セッション間でエージェントが忘れてしまうとき、永続的なプロジェクト知識が必要なとき |
| [Context Engineering](references/context-engineering-pattern.md) | コンテキスト予算の管理、JIT 読み込み、委譲の分離 |
| [Tool Registry](references/tool-registry-pattern.md) | ツールの安全性、並行制御、権限パイプライン |
| [Multi-Agent Coordination](references/multi-agent-pattern.md) | 並列化、専門化、調査者→実装者のワークフロー |
| [Lifecycle & Bootstrap](references/lifecycle-bootstrap-pattern.md) | フック、バックグラウンドタスク、初期化シーケンス |
| [Gotchas](references/gotchas.md) | 15 個の非自明な失敗モードとその対処法 |

## 使用例

### 最小ハーネスを作成する

```
ユーザー: "TypeScript プロジェクト用に AGENTS.md を整備したい"

スキルは次のことを行います。
1. プロジェクトの文脈（スタック、規模、エージェントツール）を確認する
2. 起動ワークフローと作業ルールを含む AGENTS.md を生成する
3. プレースホルダー付きの機能を含む `feature_list.json` テンプレートを作成する
4. 検証コマンド付きの `init.sh` を作成する
5. 各ファイルの使い方を説明する
```

### 既存ハーネスを評価する

```
ユーザー: "AGENTS.md があっても、エージェントがまだ壊してしまう"

スキルは次のことを行います。
1. 現在の AGENTS.md の内容を確認する
2. 5 つのサブシステムそれぞれを 1〜5 で採点する
3. 最低得点のサブシステムをボトルネックとして特定する
4. 具体的な手順を伴う優先度付き改善計画を提示する
```

### セッション継続性を設計する

```
ユーザー: "エージェントがセッションのたびにすべてを忘れてしまう"

スキルは次のことを行います。
1. メモリの層（instruction と auto-memory）の違いを説明する
2. セッション追跡用の `progress.md` テンプレートを設計する
3. `session-handoff.md` の構成を作成する
4. 2 段階保存の不変条件（topic file → index）を実装する
```

## トリガー条件

このスキルは次のような場合に使います。
- "Create AGENTS.md / CLAUDE.md"
- "Improve agent reliability"
- "Agent forgets between sessions"
- "Multi-session continuity needed"
- "Benchmark harness effectiveness"
- "Design verification workflow"
- "Memory persistence patterns"
- "Context engineering for agents"

## 対象外

このスキルは次の内容は扱いません。
- Prompt engineering や system prompt の設計
- モデル選定や fine-tuning
- 一般的なソフトウェアアーキテクチャ
- LLM API 統合の基礎

## 含まれるテンプレート

- `templates/agents.md` — 作業ルール付きの AGENTS.md スキャフォールド
- `templates/feature-list.json` — JSON Schema とサンプル
- `templates/init.sh` — 標準初期化スクリプト
- `templates/progress.md` — セッション進捗ログテンプレート
- `templates/session-handoff.md` — 引き継ぎ構成

## 評価フレームワーク

`evals/evals.json` には 5 つのテストケースがあります。
1. **Minimal Harness Creation** — ゼロからの完全セットアップ
2. **Session Continuity Setup** — メモリと引き継ぎの設計
3. **Harness Assessment** — 5 サブシステムの採点
4. **Verification Workflow Design** — 完了前に必ず検証させる
5. **Memory Taxonomy Design** — 何を保存し、何を省くか

定量ベンチマークには skill-creator フレームワークで評価を実行してください。

## 互換性

- **Agents**: Claude Code, Codex, Cursor, Windsurf, generic
- **License**: MIT
- **Languages**: English / 中文 (SKILL.md でのバイリンガル対応)

## プロジェクト構成

```
harness-creator/
├── SKILL.md                          # スキル本体の定義
├── metadata.json                     # スキルメタデータ、トリガー、互換性
├── evals/
│   └── evals.json                    # 期待値付きの 5 テストケース
├── templates/
│   ├── agents.md                     # AGENTS.md テンプレート
│   ├── feature-list.json             # 機能トラッカーテンプレート
│   ├── init.sh                       # 初期化スクリプト
│   └── progress.md                   # セッション進捗テンプレート
└── references/
    ├── memory-persistence-pattern.md
    ├── context-engineering-pattern.md
    ├── tool-registry-pattern.md
    ├── multi-agent-pattern.md
    ├── lifecycle-bootstrap-pattern.md
    └── gotchas.md                    # 15 の失敗モード
```

## 開発ロードマップ

- [x] `SKILL.md` の中国語翻訳（バイリンガル対応を追加済み）
- [ ] ハーネス自動生成用の Python スクリプト
- [ ] ハーネス評価結果の HTML ビューア
- [ ] 評価用テストケースの拡張（10 件以上）
- [ ] skill-creator ベンチマークフレームワークとの統合
- [ ] 中国語の完全ローカライズ（`harness-creator-zh` ディレクトリ）

## 貢献

Issue と PR を歓迎します。主な貢献領域は次のとおりです。
- 追加の参考パターン（skill runtime、hook lifecycle など）
- 例外ケースを含む評価テストケースの追加
- よく使うハーネスタスクのスクリプト自動化
- 実運用デプロイのケーススタディ

## ライセンス

MIT — 詳細は `LICENSE` ファイルを参照してください。

## 謝辞

このスキルは次の内容を統合しています。
- Learn Harness Engineering コースのフレームワーク
- OpenAI Harness Engineering の原則
- Anthropic の effective harnesses に関する研究
- Agentic Harness Patterns スキル（パターン抽出手法）
