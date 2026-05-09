# 英語リファレンス

このメモは、テンプレートをばらばらのファイルの寄せ集めではなく、
実際に動くハーネスとして使う方法を説明します。

## 内部リファレンスメモ

- [`method-map.md`](./method-map.md): よくある長期実行の失敗パターンを、まず対処すべき成果物やポリシーに対応づける
- [`initializer-agent-playbook.md`](./initializer-agent-playbook.md): 機能開発を始める前に、initializer が残しておくべき内容
- [`coding-agent-startup-flow.md`](./coding-agent-startup-flow.md): 後続のコーディング実行に向けた、固定のセッション開始フロー
- [`prompt-calibration.md`](./prompt-calibration.md): ルート指示を肥大化させたり脆くしたりせずに、鋭さを保つ方法

## 主要記事

この一覧は意図的に絞り込んでいます。ハーネスとは、モデルの周囲にある実行システム、
つまりエージェントループ、ツール実行、サンドボックス化、状態、コンテキスト、
検証、終了処理、オーケストレーション、可観測性を指します。一般的なプロンプト
エンジニアリングや、広い意味でのエージェントフレームワークの記事は、主要一覧には
含めません。

もともとの3本の記事が、引き続きこのコースの土台です。

- [OpenAI: Harness engineering: agent-first の世界で Codex を活用する](https://openai.com/index/harness-engineering/) (2026-02-11): agent-first のリポジトリ、repo ローカルのコンテキスト、カスタム lint、構造的なガードレール。
- [Anthropic: 長時間稼働するエージェントのための効果的なハーネス](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) (2025-11-26): initializer エージェント、coding エージェント、機能一覧、進捗ログ、コンテキストウィンドウをまたいだ引き継ぎ。
- [Anthropic: 長時間のアプリケーション開発に向けたハーネス設計](https://www.anthropic.com/engineering/harness-design-long-running-apps) (2026-03-24): planner / generator / evaluator の役割、コンテキストのリセット、ハーネスの簡素化、古い前提。

2026 年の記事は、特に関連性の高いものだけを追加しています。

- [OpenAI: Codex エージェントループを展開する](https://openai.com/index/unrolling-the-codex-agent-loop/) (2026-01-23): Codex のランタイムハーネス、ツール呼び出し、コンテキストの増大、ループの終了。
- [Anthropic: AI エージェントの eval をわかりやすく整理する](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) (2026-01-09): モデルとハーネスをまとめて評価すること、評価ハーネスとエージェントハーネスを区別すること。
- [LangChain: ハーネスエンジニアリングで Deep Agents を改善する](https://www.langchain.com/blog/improving-deep-agents-with-harness-engineering) (2026-02-17): モデルは固定したまま、system prompt、ツール、middleware、tracing、自己検証を改善して、Terminal Bench 2.0 で coding agent を Top 30 から Top 5 に押し上げる。
- [Thoughtworks / Martin Fowler: coding agent 利用者のためのハーネスエンジニアリング](https://martinfowler.com/articles/harness-engineering.html) (2026-04-02): coding-agent のユーザーハーネスを、フィードフォワードのガイドとフィードバックのセンサーとして捉え、決定論的制御と推論的制御を組み合わせる。
- [Cursor: エージェントハーネスを継続的に改善する](https://cursor.com/blog/continually-improving-agent-harness) (2026-04-30): オフライン eval、オンライン指標、ツールエラーの分類、モデル固有の調整、会話途中でのモデル切り替えを備えた、継続改善される製品システムとしてハーネスを扱う。

## 2026 拡張リファレンス

これらはコアとなるコースソースではありませんが、特定のハーネスモジュールを設計する際には
役立ちます。この節では、本文でエージェントループ、ツール実行、コンテキスト管理、
検証、サンドボックス化、制御レイヤー、回帰管理を直接扱っているソースだけを残しています。
純粋なエージェント製品、プラットフォーム発表、チーム事例、ベンチマークは除外しています。

- [OpenAI: Codex ハーネスを解き明かす: App Server をどう構築したか](https://openai.com/index/unlocking-the-codex-harness/) (2026-02-04): thread のライフサイクル、resume、fork、diff、クライアント統合を備えた再利用可能な App Server プロトコルとしてのハーネス。
- [OpenAI Developers: Codex で長期タスクを実行する](https://developers.openai.com/blog/run-long-horizon-tasks-with-codex) (2026-02-23): 長時間タスク向けの永続的なプロジェクトメモリ、マイルストーン検証、done-when の例。
- [OpenAI: Agents SDK の次の進化](https://openai.com/index/the-next-evolution-of-the-agents-sdk/) (2026-04-15): model-native なハーネス、サンドボックス実行、ファイル/コマンド実行。
- [OpenAI: Codex orchestration のオープンソース仕様: Symphony](https://openai.com/index/open-source-codex-orchestration-symphony/) (2026-04-27): issue tracker や Linear ボードをマルチエージェントの制御プレーンに変える。
- [Anthropic: 並列 Claude チームで C コンパイラを作る](https://www.anthropic.com/engineering/building-c-compiler) (2026-02-05): 並列エージェントチーム、タスクロック、git 同期、コンテナ分離、自律ループ。
- [Anthropic: Managed Agents のスケーリング: 脳と手を分離する](https://www.anthropic.com/engineering/managed-agents) (2026-04-08): セッション、ハーネス、サンドボックスを差し替え可能なインターフェースとして分けるメタハーネスの見方。
- [Anthropic: 最近の Claude Code 品質レポートの更新](https://www.anthropic.com/engineering/april-23-postmortem) (2026-04-23): 推論 effort、コンテキストの刈り込み、system prompt を、回帰管理が必要なハーネス変更として扱う。
- [LangChain: Deep Agents のコンテキスト管理](https://www.langchain.com/blog/context-management-for-deepagents) (2026-01-28): ファイルシステムへのオフロード、tool call の切り詰め、要約、コンテキスト管理ハーネス向けのターゲット評価。
- [LangChain: Deep Agents を異なるモデルでうまく動くように調整する](https://www.langchain.com/blog/tuning-deep-agents-different-models) (2026-04-29): prompt、tool 名、middleware、subagent 設定に対するモデル別ハーネスプロファイル。
- [LangChain: AI エージェントの継続学習](https://www.langchain.com/blog/continual-learning-for-ai-agents) (2026-04-05): トレースを活用して、エージェント改善を model、harness、context の各レイヤーに分ける。
- [Microsoft: Agent Framework における Agent Harness](https://devblogs.microsoft.com/agent-framework/agent-harness-in-agent-framework/) (2026-03-12): shell/filesystem ハーネス、承認フロー、ホスト型 shell 実行、コンテキスト圧縮。
- [Google: ADK for Java 1.0.0 を発表](https://developers.googleblog.com/announcing-adk-for-java-100-building-the-future-of-ai-agents-in-java/) (2026-03-30): 再利用可能なハーネス基盤としての plugins、event 圧縮、HITL、session/memory サービス、A2A。
- [GitHub: GitHub Agentic Workflows でリポジトリタスクを自動化する](https://github.blog/ai-and-ml/automate-repository-tasks-with-github-agentic-workflows/) (2026-02-13): 安全な出力、サンドボックス化、権限、レビューを備えた agentic workflow runner としての GitHub Actions。
- [AWS: 企業内の AI エージェント: Amazon Bedrock AgentCore のベストプラクティス](https://aws.amazon.com/blogs/machine-learning/ai-agents-in-enterprises-best-practices-with-amazon-bedrock-agentcore/) (2026-02-03): Runtime、Memory、Gateway、Identity/Policy、Observability、Evaluations にまたがるエンタープライズ向けハーネス層。
- [Stripe: Minions: Stripe の 1 回完結型エンドツーエンド coding agent](https://stripe.dev/blog/minions-stripes-one-shot-end-to-end-coding-agents) (2026-02-09) と [Part 2](https://stripe.dev/blog/minions-stripes-one-shot-end-to-end-coding-agents-part-2) (2026-02-19): devbox 分離、独自のエージェントハーネス、blueprint state machine、ルールファイル、MCP ツールの選別、セキュリティ制御、push 前/CI フィードバックループ。
- [Cognition: Cloud Agents を構築して学んだこと](https://cognition.ai/blog/what-we-learned-building-cloud-agents) (2026-04-23): VM 分離、セッション snapshot/resume、オーケストレーション、ガバナンス、監査ログ、cloud-agent ランタイム向け統合。
- [Cognition: マルチエージェントで実際にうまくいっていること](https://cognition.ai/blog/multi-agents-working) (2026-04-22): generator-verifier ループ、clean-context レビュアー、smart-friend ルーティング、manager-child の連携、エージェント間通信の境界。
- [Replit: Decision-Time Guidance: Replit Agent の信頼性を保つ](https://blog.replit.com/decision-time-guidance) (2026-01-20、2026-01-23 更新): 軽量な分類器が、system prompt にすべてのルールを詰め込む代わりに、判断時点で短い状況ガイダンスを挿入する。
- [Vercel: v0 を効果的な coding agent にした方法](https://vercel.com/blog/how-we-made-v0-an-effective-coding-agent) (2026-01-07): 動的な system prompt、ストリーミング rewrite レイヤー、決定論的/モデル駆動の autofixer。
- [Vercel: deepsec の紹介](https://vercel.com/blog/introducing-deepsec-find-and-fix-vulnerabilities-in-your-code-base) (2026-05-04): scan、investigate、revalidate、enrich、export、plugin、refusal-checker の手順を備えた、セキュリティ重視の coding-agent ハーネス。
- [Sourcegraph: CodeScaleBench](https://sourcegraph.com/blog/codescalebench-testing-coding-agents-on-large-codebases-and-multi-repo-software-engineering-tasks) (2026-03-03): MCP ツール採用、tool-use transcript、ベンチマーク QA、verifier/reproducibility gate、prompt/preamble の反復を扱う eval/tooling ハーネスの参考資料。

2025 年だけの一般的な参考資料は、主要一覧からは除外しています。
ただし、2025 年の Anthropic のハーネス記事は、このコースの基礎資料なので残しています。

## 推奨読書順

1. `method-map.md`
2. `initializer-agent-playbook.md`
3. `coding-agent-startup-flow.md`
4. `prompt-calibration.md`
5. OpenAI Harness engineering
6. Anthropic Effective harnesses
7. Anthropic Harness design for long-running application development
8. OpenAI Codex agent loop
9. Anthropic agent evals
10. LangChain Improving Deep Agents
11. Thoughtworks / Martin Fowler Harness engineering for coding agent users
12. Cursor Continually improving our agent harness
