---
name: harness-creator
description: >-
  AI コーディングエージェント向けの harness エンジニアリング。5 つのサブシステム、メモリ永続化、
  セッション継続、検証ワークフロー、スコープ制御、ライフサイクル管理。
when_to_use: >-
  次のような場合に使用します: ゼロから harness を構築する、エージェントの信頼性を改善する、セッション間でエージェントが忘れる、
  エージェントの暴走やスコープ逸脱、エージェント作業後のテスト破損、複数セッションの継続が必要、検証の抜け漏れ、harness 品質の監査、ベンチマーク効果測定、
  AGENTS.md/CLAUDE.md の作成、機能トラッキング設計、セッション引き継ぎ。
license: MIT
---

# Harness Creator

AI コーディングエージェント向けの本番用 harness エンジニアリング。

**対象:** コーディングエージェントの runtime、カスタムエージェント、マルチセッションのワークフローを構築・拡張するエンジニア、またはセッションをまたいでエージェントを安定動作させたい人。

**対象外:** プロンプトエンジニアリング、モデル選定、一般的なソフトウェアアーキテクチャ、単発のエージェント作業。

すべての原則は、Learn Harness Engineering フレームワークと本番エージェント runtime の判断に基づいています。

---

# Harness Creator（日本語版）

AI コーディングエージェント向けの本番用 harness エンジニアリング。

**対象:** コーディングエージェントの runtime、カスタムエージェント、マルチセッションのワークフローを構築・拡張するエンジニア、またはセッションをまたいでエージェントを安定動作させたい人。

**対象外:** プロンプトエンジニアリング、モデル選定、一般的なソフトウェアアーキテクチャ、単発のエージェント作業。

すべての原則は、Learn Harness Engineering フレームワークと本番エージェント runtime の判断に基づいています。

---

## 解決したい問題を選ぶ

| したいこと | 読む |
|---|---|
| セッション間で修正やプロジェクトルールをエージェントに覚えさせる | [Memory Persistence](references/memory-persistence-pattern.md) |
| 再利用可能なワークフローとドメイン知識をパッケージ化する | [Skill Runtime](references/skill-runtime-pattern.md) |
| エージェントを強力にしつつ危険にはしない | [Tool Registry & Safety](references/tool-registry-pattern.md) |
| 適切なコストで適切なコンテキストを与える | [Context Engineering](references/context-engineering-pattern.md) |
| 混乱なく複数のエージェントに作業を分担する | [Multi-agent Coordination](references/multi-agent-pattern.md) |
| hooks、バックグラウンドタスク、起動ロジックで動作を拡張する | [Lifecycle & Bootstrap](references/lifecycle-bootstrap-pattern.md) |
| 完全な 5 サブシステム harness を構築する | [Five Subsystems Guide](#the-five-subsystem-harness-framework) |

**構築を始める前に:** [Gotchas](#gotchas) を読んでください。これは、最も時間を浪費しやすい非自明な失敗モードです。

---

## The Five-Subsystem Harness Framework

**すべての harness は 5 つのサブシステムで構成されます。**

1. **Instructions (Recipe Shelf)**: AGENTS.md, CLAUDE.md, docs/ 階層
2. **State (Prep Station)**: feature_list.json, progress.md, session-handoff.md
3. **Verification (Quality Check Window)**: 検証コマンド、テストスイート、型チェック
4. **Scope (Task Boundaries)**: 1 機能ずつ進めるポリシー、完了条件の定義
5. **Lifecycle (Session Management)**: init.sh、クリーン状態チェックリスト、引き継ぎ手順

**harness を作成または改善するときは、各サブシステムを体系的に扱ってください。**

---

## ハーネスの作成

### フェーズ 1: コンテキスト収集

まず、ユーザーの状況を理解します。

1. **これはどのプロジェクト向けですか?**（技術スタック、規模、複雑さ）
2. **どのエージェントツールを使っていますか?**（Claude Code、Codex、Cursor など）
3. **すでに何が存在しますか?**（AGENTS.md、進捗トラッキング、検証など）
4. **どのような問題が起きていますか?**（エージェントの暴走、コンテキスト喪失、テスト破損など）
5. **チームはどの程度の構造化を許容しますか?**（最小限 vs. 包括的）

このコンテキストが提供されていない場合は、進める前に質問してください。

### フェーズ 2: harness の評価（既存プロジェクト）

ユーザーが既存の harness を持っている場合は、5 要素フレームワークで評価します。

各サブシステムを 1〜5 で採点します。
- **5**: 模範的、文書化済み、継続的に守られている
- **4**: 良好、ほぼ完成、たまに抜けがある
- **3**: 十分、基本は押さえているが仕上げが不足している
- **2**: 弱い、不完全、一貫して適用されていない
- **1**: 存在しない、または有害

最も点数の低いサブシステムを特定してください。それがボトルネックです。まずそこから改善します。

### フェーズ 3: 設計

評価結果に基づいて、harness の構成要素を設計します。

**Instructions:**
- 短い AGENTS.md（約 50〜100 行）をルーティング層として作成する
- 詳細ドキュメントを docs/ ディレクトリ（ARCHITECTURE.md、PRODUCT.md など）にリンクする
- 起動時のワークフローを定義する: コーディング前にエージェントが何を読むか

**State:**
- 機能定義と状態追跡のための feature_list.json を作成する
- セッション継続のために progress.md を作成または更新する
- 必要に応じて session-handoff.md のテンプレートを設計する

**Verification:**
- AGENTS.md に明示的な検証コマンドを記載する
- init.sh が検証を実行することを確認する
- 必要に応じて品質スコア追跡を設計する

**Scope:**
- 1 機能ずつ進めるポリシーを定義する
- 機能の依存関係を文書化する
- 完了条件チェックリストを作成する

**Lifecycle:**
- 初期化用の init.sh を作成する
- クリーン状態チェックリストを設計する
- セッション引き継ぎ手順を文書化する

### フェーズ 4: 実装

harness ファイルを作成します。利用可能なら bundled scripts を使ってください。

```bash
# scripts/ ディレクトリの bundled scripts を使う
# （利用可能なツールは scripts/ セクションを参照）
```

### フェーズ 5: テストとベンチマーク

実際のエージェントセッションで harness をテストします。

1. **ベースライン**: harness なしで代表的なタスクを実行する
2. **harness あり**: 同じタスクを harness ありで実行する
3. **計測**: 成功率、所要時間、トークン使用量、手戻り
4. **比較**: 改善を定量化する

厳密なベンチマークについては、以下の「ベンチマークの実行」セクションを参照してください。

---

## Harness ファイルのテンプレート

### AGENTS.md の構成

最小限の AGENTS.md には次を含めます。

```markdown
# AGENTS.md

[1 行で書くプロジェクトの目的]

## Startup Workflow

コードを書く前に:
1. [ステップ 1: 例、このファイルを読む]
2. [ステップ 2: 例、ARCHITECTURE.md を読む]
3. [ステップ 3: 例、./init.sh を実行する]
4. [ステップ 4: 例、feature_list.json を読む]

## Working Rules

- [ルール 1: 例、一度に 1 機能だけ扱う]
- [ルール 2: 例、完了を主張する前に検証を必須にする]
- [ルール 3: 例、セッション終了前に progress を更新する]

## Required Artifacts

- `feature_list.json`: 機能状態トラッカー
- `progress.md`: セッション継続ログ
- `init.sh`: 標準の起動と検証

## Definition of Done

機能が完了する条件:
- [ ] 実装完了
- [ ] 検証成功
- [ ] 証跡を記録済み
- [ ] リポジトリを再起動可能

## End of Session

終了前に:
1. progress.md を更新する
2. feature_list.json を更新する
3. ブロッカー / リスクを記録する
4. 説明的なコミットメッセージで commit する
5. クリーンに再開できる状態を残す
```

### feature_list.json の構成

```json
{
  "features": [
    {
      "id": "feat-001",
      "name": "Document Import",
      "description": "PDF と TXT の文書をインポートできるようにする",
      "dependencies": [],
      "status": "done",
      "evidence": "tests pass, manual verification on 2024-01-15"
    },
    {
      "id": "feat-002",
      "name": "Document Chunking",
      "description": "メタデータ付きで文書を約 500 文字ごとのチャンクに分割する",
      "dependencies": ["feat-001"],
      "status": "in-progress",
      "evidence": ""
    }
  ]
}
```

### init.sh の構成

```bash
#!/bin/bash
set -e

echo "=== Installing dependencies ==="
npm install

echo "=== Running type check ==="
npm run check

echo "=== Running tests ==="
npm test

echo "=== Building application ==="
npm run build

echo "=== Verification complete ==="
```

---

## ベンチマークの実行

harness の有効性を測定するには:

### ステップ 1: 代表的なタスクを定義する

次の条件を満たすタスクを 2〜3 件選びます。
- ユーザーが実際に行う本物の作業である
- 適切な harness がないと失敗しうる程度に難しい
- 検証可能である（成功条件が明確）

### ステップ 2: 比較セッションを実行する

各タスクについて:
- **harness なし**: クリーンなリポジトリコピーでタスクを実行する
- **harness あり**: harness を入れた状態で同じタスクを実行する

記録する項目:
- 成功 / 失敗
- 所要時間
- トークン使用量
- 必要な手戻り
- 必要になったセッション再開回数

### ステップ 3: 結果を集計する

次を算出します。
- 成功率の改善
- 時間効率の変化
- トークン効率の変化
- 定性的なフィードバック

### ステップ 4: 繰り返し改善する

結果から次を特定します。
- どの harness コンポーネントが最も価値を生むか
- どのコンポーネントが過剰設計か
- どこに改善努力を集中すべきか

---

## 付属リソース

### 参考資料（詳細パターン）

| Document | Covers |
|----------|--------|
| [Memory Persistence](references/memory-persistence-pattern.md) | 4 層の指示階層、自動メモリ分類、バックグラウンド抽出 |
| [Context Engineering](references/context-engineering-pattern.md) | Select / Compress / Isolate / Write 操作、予算管理 |
| [Tool Registry](references/tool-registry-pattern.md) | fail-closed 登録、呼び出しごとの並行性、権限パイプライン |
| [Multi-Agent](references/multi-agent-pattern.md) | Coordinator / Fork / Swarm パターン、コンテキスト共有 |
| [Lifecycle & Bootstrap](references/lifecycle-bootstrap-pattern.md) | hook システム、長時間実行タスク、依存順の init |
| [Gotchas](references/gotchas.md) | 修正付きの 15 の非自明な失敗モード |

### テンプレート

- `templates/agents.md` — AGENTS.md / CLAUDE.md のひな形
- `templates/feature-list.json` — 機能状態トラッカー
- `templates/init.sh` — 標準初期化スクリプト
- `templates/progress.md` — セッション進捗ログ
- `templates/session-handoff.md` — セッション引き継ぎテンプレート

### スクリプト（任意）

- `scripts/create-harness.ts` — テンプレートから harness ファイルを生成する
- `scripts/validate-harness.ts` — harness の完全性をチェックする
- `scripts/run-benchmark.ts` — harness の有効性比較を実行する

---

## Gotchas

違反すると bug の原因になる、非自明な原則です。

1. **メモリのインデックス上限は静かに発火する** — 長いエントリは上限に達すると見えなくなる。hook は 1 行に保つ。
2. **優先順位の順序は直感に反する** — ローカルがプロジェクトより上、プロジェクトがユーザーより上、ユーザーが組織より上。スタック全体でテストする。
3. **抽出タイミングが競合窓を生む** — バックグラウンド抽出が完了する前に、ユーザーが次のターンを始められる。
4. **推論可能な内容はメモリに入れない** — アーキテクチャやコードパターンはすでにリポジトリ内にある。
5. **並列分類はツール単位ではなく呼び出し単位** — 同じツールでも入力によって安全な場合と危険な場合がある。
6. **権限評価には副作用がある** — 拒否の追跡、モード変換、状態更新を行う。
7. **ほとんどの非同期作業は "pending" を飛ばす** — work unit は直接 "running" として登録される。
8. **Fork した子はさらに Fork してはいけない** — 再帰ガードが単一レベル不変条件を守る。
9. **context builder はメモ化されるが手動で無効化される** — 無効化を追加しないと古い内容になる。
10. **hook の信頼は全有無** — 1 つでも信頼できない hook があると拡張システム全体が無効になる。
11. **eviction には通知が必要** — 終端 work unit は親に通知されるまで GC 対象にならない。
12. **Skill の一覧バジェットは厳しい** — 目立つトリガー語を先頭に置き、末尾は切り捨てられる。

**完全なガイド:** [Gotchas](references/gotchas.md) — 修正付きの 15 の失敗モード。

## 罠（Gotchas）

違反すると bug の原因になる、非自明な原則です。

1. **メモリのインデックス上限は静かに発火する** — 長いエントリは上限に達すると見えなくなる。hook は 1 行に保つ。
2. **優先順位の順序は直感に反する** — ローカルがプロジェクトより上、プロジェクトがユーザーより上、ユーザーが組織より上。スタック全体でテストする。
3. **抽出タイミングが競合窓を生む** — バックグラウンド抽出が完了する前に、ユーザーが次のターンを始められる。
4. **推論可能な内容はメモリに入れない** — アーキテクチャやコードパターンはすでにリポジトリ内にある。
5. **並列分類はツール単位ではなく呼び出し単位** — 同じツールでも入力によって安全な場合と危険な場合がある。
6. **権限評価には副作用がある** — 拒否の追跡、モード変換、状態更新を行う。
7. **ほとんどの非同期作業は "pending" を飛ばす** — work unit は直接 "running" として登録される。
8. **Fork した子はさらに Fork してはいけない** — 再帰ガードが単一レベル不変条件を守る。
9. **context builder はメモ化されるが手動で無効化される** — 無効化を追加しないと古い内容になる。
10. **hook の信頼は全有無** — 1 つでも信頼できない hook があると拡張システム全体が無効になる。
11. **eviction には通知が必要** — 終端 work unit は親に通知されるまで GC 対象にならない。
12. **Skill の一覧バジェットは厳しい** — 目立つトリガー語を先頭に置き、末尾は切り捨てられる。

**完全なガイド:** [Gotchas](references/gotchas.md) — 修正付きの 15 の失敗モード。

---

## この Skill を使う場面

次のような場合にこの skill を使います。

- ユーザーが「プロジェクト用に AGENTS.md を設定したい」と言っている
- ユーザーがエージェントの信頼性を改善したい
- ユーザーがエージェントの失敗、コンテキスト喪失、作業破損に悩んでいる
- ユーザーが「エージェントをもっと良く動かすには?」と尋ねている
- ユーザーが harness の有効性をベンチマークしたい
- ユーザーが harness ファイルのテンプレートを必要としている
- ユーザーが Learn Harness Engineering コースに沿って進めている

---

## コミュニケーション スタイル

- harness の概念は実用的な言葉で説明する（キッチンのたとえが有効）
- 理論上の完全性ではなく、測定可能な成果に焦点を当てる
- 最小構成から始め、必要に応じて構造を追加する
- before/after の比較を示して信頼を高める
- トレードオフを明示する（構造が増えるほど信頼性は上がるが、初期作業も増える）

---

## 始め方

### ユーザーが harness engineering に不慣れな場合:

1. **まず評価する**: 現在のセットアップで 5 要素評価を実施する
2. **最も低いサブシステムを選ぶ**: まずそこに改善努力を集中する
3. **最小実用 harness を作る**: AGENTS.md + init.sh + feature_list.json
4. **実タスクで試す**: 改善の前後を測定する

### ユーザーが経験豊富な場合:

1. **具体的な問題を聞く**: 推測しない。痛点を説明してもらう
2. **harness の成熟度を把握する**: すでに何があり、何が機能しているか
3. **的を絞った改善を設計する**: 参考パターンを手がかりにする
4. **必要ならベンチマークを実行する**: before/after 比較で効果を定量化する

---

## この Skill を使わない場面

この skill はエージェントの周辺にある **harness** を扱うものであり、次のものではありません。
- プロンプトエンジニアリングや system prompt の設計
- モデル選定や fine-tuning
- 一般的なソフトウェアアーキテクチャ（MVC、microservices）
- Chat UI や会話インターフェース
- LLM API 統合の基礎

モデルそのものではなく、その周辺システムについての質問でないなら、この skill は該当しません。

---

## 参考情報

- [Learn Harness Engineering Documentation](https://walkinglabs.github.io/learn-harness-engineering/)
- [OpenAI: Harness Engineering](https://openai.com/index/harness-engineering/)
- [Anthropic: Effective Harnesses for Long-Running Agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents/)
- [Anthropic: Harness Design for Long-Running Apps](https://www.anthropic.com/engineering/harness-design-long-running-apps)
- [Awesome Harness Engineering](https://github.com/walkinglabs/awesome-harness-engineering)
- [Agentic Harness Patterns Skill](https://github.com/keli-wen/agentic-harness-patterns-skill) — パターン抽出の参考実装

---

## さらに詳しい情報

- [Learn Harness Engineering Documentation](https://walkinglabs.github.io/learn-harness-engineering/)
- [OpenAI: Harness Engineering](https://openai.com/index/harness-engineering/)
- [Anthropic: Effective Harnesses for Long-Running Agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents/)
- [Awesome Harness Engineering](https://github.com/walkinglabs/awesome-harness-engineering)
