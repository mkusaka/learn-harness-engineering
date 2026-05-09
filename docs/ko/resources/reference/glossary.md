---
title: 日本語用語集 (Glossary)
description: Learn Harness Engineering 日本語版全体で使う主要用語の英日対応表と定義です。
---

# 日本語用語集 (Glossary)

この文書は、日本語版全体で一貫した訳語を保証するための単一の信頼できる情報源 (Single Source of Truth) です。各講義・プロジェクト・リソース文書で最初に現れる専門用語は、この用語集の表記に従います。

## 表記規則 (Convention)

- **初出形式**: 本文で最初に出てくる専門用語は `日本語(English)` の形で併記します。例: `ハーネス(harness)`, `エージェント(agent)`。
- **2回目以降**: 日本語のみを使いますが、文脈上あいまいな場合は再び英語を併記できます。
- **固有名詞・ツール名**: `Claude Code`, `Codex`, `VitePress`, `GitHub Actions` などは英語のままにします。
- **システムトークン**: `BLOCKING`, `HARD`, `[CP]`, `@plan-writer`, `Skill("...")` など、コードやプロンプトで意味を持つトークンは翻訳しません。
- **コマンド・ファイルパス**: `npm run docs:build`, `docs/.vitepress/config.mts` などは英語のまま保持します。

## 核心概念 (Core Concepts)

| English | 日本語 | 定義 |
|---------|--------|------|
| Harness | ハーネス | AI コーディングエージェントが安定して動作できるように、環境・状態・検証・制御をまとめて提供する外骨格 (scaffolding) システム。本講義の中心概念。 |
| Harness Engineering | ハーネスエンジニアリング | ハーネスを設計・構築・保守する工学的実践領域。 |
| Agent | エージェント | 自律的に作業を進める AI ソフトウェア単位。本講義では主にコーディングエージェント (coding agent) を指します。 |
| Coding Agent | コーディングエージェント | コード作成・リファクタ・テストなどの開発作業を行う AI エージェント。 |
| State | 状態 | エージェントが次の行動を決める際に参照する永続データ (作業進捗ログ、機能一覧、git 履歴、チェックリストなど)。 |
| Context | コンテキスト | エージェントが1回の推論で使う入力情報の集合。モデルのコンテキストウィンドウ (context window) に入る内容。 |
| Context Window | コンテキストウィンドウ | LLM が一度に処理できるトークンの最大長。 |
| Repository as System of Record | システムオブレコード (SoR) としてのリポジトリ | 作業結果・判断・根拠をすべて git リポジトリに永続化し、信頼の単一基準とする原則。 |

## 作業・成果物 (Work and Deliverables)

| English | 日本語 | 定義 |
|---------|--------|------|
| Deliverable | 成果物 | 作業の結果として作られる、ファイル・文書・コードなど検証可能な実体。 |
| Specification | 仕様 | 作業の入力・動作・出力を明確に定義した文書。`spec` と略すこともあります。 |
| Plan | 計画 | 仕様を満たすための段階的な作業順序と責任分担をまとめた文書。 |
| TODO Checklist | TODO チェックリスト | 計画を実行可能な単位作業に分解したチェックボックス一覧。 |
| Commit | コミット | git で変更を永続化する単位。 |
| Branch | ブランチ | 独立した作業の流れを維持するための git の分岐。 |
| Worktree | ワークツリー | 同じリポジトリで複数の作業を同時に進めるための別作業ディレクトリ。 |
| Pull Request (PR) | プルリクエスト (PR) | あるブランチの変更を別のブランチへ取り込むよう依頼する GitHub の協業単位。 |

## 検証と品質 (Verification and Quality)

| English | 日本語 | 定義 |
|---------|--------|------|
| Verification | 検証 | 成果物が仕様を満たすかを客観的な証拠で確かめる手順。 |
| Validation | 妥当性確認 | 入力値が制約を満たすかを確認する手順。検証より狭い意味です。 |
| Critique | クリティーク | 計画・設計・成果物の弱点を事前に見つけるための対立的 (adversarial) レビュー。 |
| Review | レビュー | コードや文書を同僚またはエージェントが読み、フィードバックを残す手順。 |
| Test | テスト | コードの動作を自動で確認する検証手段 (単体・統合・E2E など)。 |
| TDD (Test-Driven Development) | テスト駆動開発 | RED→GREEN→IMPROVE の順で、先にテストを書いてから実装する手法。 |
| Acceptance Criteria | 受け入れ基準 | 作業が完了したと認められるために満たすべき客観的条件。 |
| Quality Gate | 品質ゲート | 次の段階へ進む前に通過しなければならない、自動・手動のチェック群。 |
| Lint | リント | 静的解析でスタイルや潜在的なエラーを検出するツール (例: `ruff`, `eslint`)。 |
| Type Check | 型チェック | 静的型システムで型エラーを事前に検出する手順 (例: `pyright`, `tsc --noEmit`)。 |

## ハーネス構成要素 (Harness Components)

| English | 日本語 | 定義 |
|---------|--------|------|
| Skill | スキル | 特定の作業フローを表した再利用可能なプロンプトモジュール。必要に応じてエージェントが読み込みます。 |
| Hook | フック | エージェントのライフサイクル (例: ツール呼び出し前後、セッション開始) のタイミングで実行されるユーザー定義スクリプト。 |
| Rule | ルール | エージェントが常に従うべき方針・制約をまとめた Markdown 文書。 |
| Subagent / Agent (role) | サブエージェント / 役割エージェント | メインセッションが委譲できる専門ロール (例: `@api-implement`, `@code-review`)。 |
| Orchestration | オーケストレーション | 複数のエージェント・ツール・検査を決められた順序でまとめて実行する上位のフロー制御。 |
| Permission | 権限 | エージェントが特定のツール・ファイル・コマンドを使えるかを制御する設定。 |
| Preflight | 事前点検 (preflight) | 本作業を始める前に、環境や前提条件が整っているかを確認する段階。 |
| Guardrail | ガードレール | エージェントが危険な行動を取れないようにする方針・検証の仕組み。 |
| Telemetry | テレメトリ | エージェント実行中に収集する測定指標 (イベントログ、遅延時間、失敗率など)。 |

## 失敗と復旧 (Failure and Recovery)

| English | 日本語 | 定義 |
|---------|--------|------|
| Fail-loud | 失敗を大きく知らせる (fail-loud) | エラーが起きたら、すぐに目立つ形で通知する方針。静かな失敗の反対です。 |
| Silent Failure | サイレント失敗 | エラーが起きても何の兆候もなく次の段階へ進んでしまう現象。 |
| Continuity | 継続性 | 長時間の作業中でもコンテキストや状態が保たれ、途切れない性質。 |
| Handoff | ハンドオフ | あるセッションまたはあるエージェントから次のセッション・エージェントへ作業を引き継ぐ行為。 |
| Session Handoff | セッションハンドオフ | コンテキストが切れる危険があるとき、次のセッションが引き継げるように状態を永続化するパターン。 |
| Checkpoint | チェックポイント | 復旧可能な進行点で状態を永続化した時点。 |
| Rollback | ロールバック | 特定の時点まで状態を戻す行為。 |

## 初期化とコンベンション (Initialization and Conventions)

| English | 日本語 | 定義 |
|---------|--------|------|
| Initialization | 初期化 | 新しいプロジェクトや作業環境でハーネスを設定する段階。 |
| Onboarding | オンボーディング | 新しいメンバー (人またはエージェント) がプロジェクトのコンベンションを学び、適用できるように支援する手順。 |
| Convention | コンベンション | チームで合意した命名・構造・作業方法の標準。 |
| Bootstrap | ブートストラップ | 何もない状態から動作可能な最小環境を整えること。 |
| Scaffold | スキャフォールド | 作業の出発点となるディレクトリやファイル構造をあらかじめ作成するツール・パターン。 |

## ツールとエコシステム (Tools and Ecosystem)

| English | 日本語(またはそのまま) | 備考 |
|---------|---------------------|------|
| Claude Code | Claude Code | そのまま使用。Anthropic のコーディングエージェント CLI。 |
| Codex | Codex | そのまま使用。OpenAI のコーディングエージェント CLI。 |
| Cursor | Cursor | そのまま使用。AI コードエディタ。 |
| VitePress | VitePress | そのまま使用。Markdown ベースの静的サイトジェネレーター。 |
| MCP (Model Context Protocol) | MCP (Model Context Protocol) | エージェントが外部ツール・リソースを発見・呼び出すための標準。 |
| LSP (Language Server Protocol) | LSP (Language Server Protocol) | エディタと言語ツールがコード情報をやり取りするための標準。 |
| GitHub Actions | GitHub Actions | そのまま使用。GitHub の CI/CD システム。 |
| LangGraph | LangGraph | そのまま使用。状態グラフベースのエージェントフレームワーク。 |

## 保持トークン (Untranslated Tokens)

次のトークンは、日本語版本文でも絶対に翻訳せず、英語のまま保持します。

- システム意味トークン: `BLOCKING`, `HARD`, `[CP]`, `RED`, `GREEN`, `IMPROVE`, `DONE`, `RETRY`, `BLOCKED`
- エージェント識別子: `@plan-writer`, `@code-review`, `@doc-writer`, `@ui-implement` など、すべての `@agent-name`
- スキル呼び出し: `Skill("omb-doc")`, `Skill("omb-tdd")` など、すべての `Skill("...")` 形式
- ファイルパス・コマンド: `docs/.vitepress/config.mts`, `npm run docs:build`, `git commit -m "..."` など
- frontmatter キー: `title`, `description`, `layout` など、`---` ブロック内の英語キー名

## 今後の更新 (Maintenance)

講義の翻訳中に新しい用語が見つかったら、この文書にすぐ追加し、後続の講義翻訳では更新済みの内容を入力として使います (計画 Task #16b 参照)。
