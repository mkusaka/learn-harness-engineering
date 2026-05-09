# 日本語リファレンス

このセクションでは、これらのテンプレートを孤立したファイルの寄せ集めとしてではなく、どう組み合わせて使うかを説明します。

## 内部参考資料

- [`method-map.md`](./method-map.md)：よくある長時間タスクの失敗ポイントを、対応する方法や成果物に対応づける
- [`initializer-agent-playbook.md`](./initializer-agent-playbook.md)：初期化エージェントが最初のラウンドで何を出すべきか
- [`coding-agent-startup-flow.md`](./coding-agent-startup-flow.md)：後続のコーディングエージェントが毎回着手時に踏む固定フロー
- [`prompt-calibration.md`](./prompt-calibration.md)：ルート指示をどこまで具体化するのが適切か

## 重点参考記事

ここでの選定基準はかなり厳しく、harness の仕組みを直接説明している記事だけを残しています。ここでいう harness とは、モデルの外側で動く実行システム全体を指し、agent loop、ツール実行、サンドボックス、状態、コンテキスト、検証、終了条件、制御プレーン、観測フィードバックを含みます。一般的な prompt engineering や agent フレームワークの紹介ではありません。

元の3本を授業の主軸として残します。

- [OpenAI: Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/)（2026-02-11）：agent-first のリポジトリ、repo-local context、custom lint、構造的な guardrail。
- [Anthropic: Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)（2025-11-26）：initializer agent、coding agent、feature list、progress log、コンテキストウィンドウをまたぐ引き継ぎ。
- [Anthropic: Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps)（2026-03-24）：planner / generator / evaluator の3役割、context reset、harness の簡素化、コンポーネントの陳腐化問題。

さらに、関連性が高く価値の大きい 2026 年の記事だけを追加します。

- [OpenAI: Unrolling the Codex agent loop](https://openai.com/index/unrolling-the-codex-agent-loop/)（2026-01-23）：Codex runtime harness の中核ループ、ツール呼び出し、コンテキストの増大、終了状態を解説。
- [Anthropic: Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)（2026-01-09）：agent を評価する際には model + harness を評価していることを明確にし、evaluation harness と agent harness を区別する。
- [LangChain: Improving Deep Agents with harness engineering](https://www.langchain.com/blog/improving-deep-agents-with-harness-engineering)（2026-02-17）：モデルは同じまま、system prompt、tools、middleware、tracing、self-verification だけを変えて、Terminal Bench 2.0 で coding agent を Top 30 から Top 5 へ押し上げる。
- [Thoughtworks / Martin Fowler: Harness engineering for coding agent users](https://martinfowler.com/articles/harness-engineering.html)（2026-04-02）：coding-agent user harness を feedforward guides と feedback sensors に分け、deterministic controls と inferential controls を区別する。
- [Cursor: Continually improving our agent harness](https://cursor.com/blog/continually-improving-agent-harness)（2026-04-30）：harness を継続的に改善する製品システムとして捉え、オフライン評価、オンライン指標、ツールエラー分類、モデルのカスタマイズ、mid-chat model switching で agent の挙動を改善する。

## 2026 年の拡張参考

これらの記事は授業の主軸ではありませんが、特定の harness モジュールを設計するときに大いに参考になります。記事本文のうち、agent loop、ツール実行、コンテキスト管理、検証、サンドボックス、制御層、回帰ガバナンスに直接触れている部分だけを残し、純粋な agent 製品、プラットフォーム発表、チーム実践、ベンチマークはここに含めません。

- [OpenAI: Unlocking the Codex harness: how we built the App Server](https://openai.com/index/unlocking-the-codex-harness/)（2026-02-04）：harness を App Server プロトコルとして抽象化し、thread lifecycle、resume、fork、diff、クライアント統合を扱う。
- [OpenAI Developers: Run long horizon tasks with Codex](https://developers.openai.com/blog/run-long-horizon-tasks-with-codex)（2026-02-23）：長期タスクにおける durable project memory、milestone validation、done-when の例。
- [OpenAI: The next evolution of the Agents SDK](https://openai.com/index/the-next-evolution-of-the-agents-sdk/)（2026-04-15）：model-native harness、sandbox execution、ファイルとコマンドの実行機能。
- [OpenAI: An open-source spec for Codex orchestration: Symphony](https://openai.com/index/open-source-codex-orchestration-symphony/)（2026-04-27）：issue tracker / Linear を多 agent の制御プレーンに変える。
- [Anthropic: Building a C compiler with a team of parallel Claudes](https://www.anthropic.com/engineering/building-c-compiler)（2026-02-05）：並列 agent チーム、タスクロック、git 同期、コンテナ分離、自律ループ。
- [Anthropic: Scaling Managed Agents: Decoupling the brain from the hands](https://www.anthropic.com/engineering/managed-agents)（2026-04-08）：meta-harness の観点から、session、harness、sandbox を差し替え可能なインターフェースとして分割する。
- [Anthropic: An update on recent Claude Code quality reports](https://www.anthropic.com/engineering/april-23-postmortem)（2026-04-23）：reasoning effort、context pruning、system prompt はすべて harness 変更であり、回帰ガバナンスが必要になる。
- [LangChain: Context Management for Deep Agents](https://www.langchain.com/blog/context-management-for-deepagents)（2026-01-28）：filesystem offloading、tool-call truncation、summarization、targeted evals から成る context-management harness。
- [LangChain: Tuning Deep Agents to Work Well with Different Models](https://www.langchain.com/blog/tuning-deep-agents-different-models)（2026-04-29）：model-specific harness profiles を使って prompt、tool names、middleware、subagent 設定を調整する。
- [LangChain: Continual learning for AI agents](https://www.langchain.com/blog/continual-learning-for-ai-agents)（2026-04-05）：agent 改善を model、harness、context の3層に分け、traces を改善シグナルとして扱う。
- [Microsoft: Agent Harness in Agent Framework](https://devblogs.microsoft.com/agent-framework/agent-harness-in-agent-framework/)（2026-03-12）：shell/filesystem harness、approval flow、hosted shell、context compaction。
- [Google: Announcing ADK for Java 1.0.0](https://developers.googleblog.com/announcing-adk-for-java-100-building-the-future-of-ai-agents-in-java/)（2026-03-30）：プラグイン、event compaction、HITL、session/memory service、A2A など、再利用可能な harness primitives。
- [GitHub: Automate repository tasks with GitHub Agentic Workflows](https://github.blog/ai-and-ml/automate-repository-tasks-with-github-agentic-workflows/)（2026-02-13）：GitHub Actions を agentic workflow runner に変え、safe outputs、sandboxing、permissions、review を含める。
- [AWS: AI agents in enterprises: Best practices with Amazon Bedrock AgentCore](https://aws.amazon.com/blogs/machine-learning/ai-agents-in-enterprises-best-practices-with-amazon-bedrock-agentcore/)（2026-02-03）：Runtime、Memory、Gateway、Identity/Policy、Observability、Evaluations の企業向け harness 階層。
- [Stripe: Minions: Stripe's one-shot, end-to-end coding agents](https://stripe.dev/blog/minions-stripes-one-shot-end-to-end-coding-agents)（2026-02-09）と [Part 2](https://stripe.dev/blog/minions-stripes-one-shot-end-to-end-coding-agents-part-2)（2026-02-19）：devbox 分離、custom agent harness、blueprints 状態機械、ルールファイル、MCP tool curation、安全制御、pre-push/CI フィードバックループ。
- [Cognition: What We Learned Building Cloud Agents](https://cognition.ai/blog/what-we-learned-building-cloud-agents)（2026-04-23）：クラウド上の agent runtime における VM 分離、session snapshot/resume、orchestration、governance、audit logging、integrations。
- [Cognition: Multi-Agents: What's Actually Working](https://cognition.ai/blog/multi-agents-working)（2026-04-22）：generator-verifier loop、clean-context reviewer、smart-friend routing、manager-child coordination、agent 間通信の境界。
- [Replit: Decision-Time Guidance: Keeping Replit Agent Reliable](https://blog.replit.com/decision-time-guidance)（2026-01-20、2026-01-23 更新）：すべてのルールを system prompt に詰め込まず、軽量分類器で重要な意思決定点に短い指示を差し込む。
- [Vercel: How we made v0 an effective coding agent](https://vercel.com/blog/how-we-made-v0-an-effective-coding-agent)（2026-01-07）：動的 system prompt、streaming rewrite layer、deterministic/model-driven autofixers。
- [Vercel: Introducing deepsec](https://vercel.com/blog/introducing-deepsec-find-and-fix-vulnerabilities-in-your-code-base)（2026-05-04）：セキュリティスキャン向けの coding-agent harness で、scan、investigate、revalidate、enrich、export、plugin、refusal-checker を含む。
- [Sourcegraph: CodeScaleBench](https://sourcegraph.com/blog/codescalebench-testing-coding-agents-on-large-codebases-and-multi-repo-software-engineering-tasks)（2026-03-03）：eval/tooling harness 寄りで、MCP tool adoption、tool-use transcripts、benchmark QA、verifier/reproducibility gates、prompt/preamble の反復を含む。

厳密に時系列で絞ると、2025 年だけの一般的な参考記事は主リストに入りません。元の3本のうち Anthropic の 2025 年記事を残しているのは、このコースの手法の基礎的な出典だからです。

## 推奨読む順

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
