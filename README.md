[英語版](https://walkinglabs.github.io/learn-harness-engineering/en/) · [中国語版](https://walkinglabs.github.io/learn-harness-engineering/zh/) · [ロシア語版](https://walkinglabs.github.io/learn-harness-engineering/ru/) · [ベトナム語版](https://walkinglabs.github.io/learn-harness-engineering/vi/) · [韓国語版](https://walkinglabs.github.io/learn-harness-engineering/ko/)

# Learn Harness Engineering

> **AI コーディングエージェントを安定して動かすための、環境構築・状態管理・検証・制御機構を学ぶプロジェクトベースの講座です。**

Learn Harness Engineering は、AI コーディングエージェント向けのエンジニアリングに特化した講座です。私たちは、業界の最前線にある harness engineering の理論と実践を深く研究し、それらを統合しています。主な参照元は次のとおりです。

- [OpenAI: Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/)
- [Anthropic: Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
- [Anthropic: Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps)
- [Awesome Harness Engineering](https://github.com/walkinglabs/awesome-harness-engineering)

> **すぐに始めたいですか？** [`skills/harness-creator/`](./skills/) の skill を使えば、自分のプロジェクト向けに本番レベルの harness（`AGENTS.md`、機能一覧、`init.sh`、検証ワークフロー）を数分で組み立てられます。

---

## 目次

- [✨ ビジュアルプレビュー](#-visual-preview)
- [Harness Engineering とは何か](#what-harness-engineering-actually-means)
- [クイックスタート: 今すぐエージェントを改善する](#quick-start-improve-your-agent-today)
- [キャップストーンプロジェクト: 実アプリ](#capstone-project-a-real-app)
- [学習パス](#learning-path)
- [シラバス](#syllabus)
- [Skills](#skills)
- [他の講座](#other-courses)

---

<a id="-visual-preview"></a>
## ✨ ビジュアルプレビュー

### 🏠 コースホームページ
> コース全体の構成と中核となる考え方をまとめた、学習の入口がひと目で分かるページです。

![コースホームページのプレビュー](./docs/public/screenshots/readme/en-home.png)

### 📖 没入型の講義
> 実際の課題や、Project 01 のようなハンズオンプロジェクトを深く掘り下げた、没入感のある学習体験を提供します。

![講義プレビュー](./docs/public/screenshots/readme/en-lecture-01.png)

### 🗂️ すぐ使えるリソースライブラリ
> コンテキスト喪失やタスクの早すぎる完了といった、マルチターンの AI エージェント開発でよく起きる落とし穴を避けるために設計されたテンプレートと参照設定をまとめています。

![リソースライブラリのプレビュー](./docs/public/screenshots/readme/en-resources.png)

## PDF コースブック

このリポジトリには、講義内容を PDF に出力するパイプラインが含まれています。

- `npm run pdf:build` を実行すると、英語版と中国語版の PDF をローカル生成できます。
- 出力先は `artifacts/pdfs/` です。
- README のプレビュー画像を更新したい場合は `npm run screenshots:readme` を実行してください。
- GitHub Actions のワークフロー [`release-course-pdfs.yml`](./.github/workflows/release-course-pdfs.yml) は、PDF をビルドして GitHub Releases に公開できます。

---

## モデルは賢い。ハーネスがそれを信頼できるものにする。

多くの人が痛みを伴って学ぶ厳しい事実があります。**世界で最も強力なモデルでも、適切な環境を周囲に用意しなければ、実際のエンジニアリング作業では失敗します。**

きっとあなたも似た経験があるはずです。リポジトリ上で Claude や GPT にタスクを与えると、最初は順調に進みます。ファイルを読み、コードを書き、うまく動いているように見えます。ところが、どこかで崩れます。手順を飛ばす。テストを壊す。「完了しました」と言うのに、実際には何も動かない。自分でやるより、後始末のほうに時間がかかります。

これはモデルの問題ではありません。ハーネスの問題です。

証拠は明確です。Anthropic は制御実験を行いました。モデルは同じ（Opus 4.5）、プロンプトも同じ（「2D レトロゲームエディタを作る」）。ハーネスなしでは、20 分で 9 ドルを消費し、動かないものしか作れませんでした。完全なハーネス（planner + generator + evaluator）を使うと、6 時間で 200 ドルを消費し、実際に遊べるゲームを構築しました。モデルは変わっていません。変わったのはハーネスです。

OpenAI も Codex で同じことを報告しています。よく整備されたハーネスのあるリポジトリでは、同じモデルが「不安定」から「信頼できる」へと変わります。単なる微調整ではなく、質的な変化です。

**この講座では、その環境をどう作るかを学びます。**

```text
                    ハーネスの基本パターン
                    ======================

    あなた --> タスクを与える --> Agent が harness ファイルを読む --> Agent が実行
                                                        |
                                              harness が各ステップを統制する:
                                              |
                                              +--> Instructions: 何を、どの順番でやるか
                                              +--> Scope:       1 つの機能ずつ、過剰拡張なし
                                              +--> State:       進捗ログ、機能一覧、git 履歴
                                              +--> Verification: tests、lint、type-check、smoke 実行
                                              +--> Lifecycle:   最初に init、最後にクリーンな状態へ
                                              |
                                              v
                                         検証に合格するまで
                                         Agent は止まらない
```

---

<a id="what-harness-engineering-actually-means"></a>
## Harness Engineering とは何か

harness engineering とは、モデルの周囲に完全な実行環境を作り、安定した結果を出せるようにすることです。より良いプロンプトを書くことではありません。モデルが動くシステムそのものを設計することです。

ハーネスには 5 つのサブシステムがあります。

```text
    ┌─────────────────────────────────────────────────────────────────┐
    │                         ハーネス                               │
    │                                                                 │
    │   ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐  │
    │   │ Instructions  │  │    State     │  │   Verification      │  │
    │   │              │  │              │  │                      │  │
    │   │ AGENTS.md    │  │ progress.md  │  │ tests + lint         │  │
    │   │ CLAUDE.md    │  │ feature_list │  │ type-check           │  │
    │   │ feature_list │  │ git log      │  │ smoke runs           │  │
    │   │ docs/        │  │ session hand │  │ e2e pipeline         │  │
    │   └──────────────┘  └──────────────┘  └──────────────────────┘  │
    │                                                                 │
    │   ┌──────────────┐  ┌──────────────────────────────────────┐    │
    │   │    Scope     │  │         Session Lifecycle            │    │
    │   │              │  │                                      │    │
    │   │ one feature  │  │ init.sh at start                     │    │
    │   │ at a time   │  │ clean-state checklist at end          │    │
    │   │ definition   │  │ handoff note for next session        │    │
    │   │ of done      │  │ commit only when safe to resume      │    │
    │   └──────────────┘  └──────────────────────────────────────┘    │
    │                                                                 │
    └─────────────────────────────────────────────────────────────────┘

    モデルは書くコードを決める。
    ハーネスは、いつ、どこで、どのように書くかを統制する。
    ハーネスはモデルを賢くするわけではない。
    モデルの出力を信頼できるものにする。
```

それぞれのサブシステムには、明確な役割があります。

- **Instructions** — エージェントに、何を、どの順番で、開始前に何を読むべきかを伝えます。巨大な 1 ファイルではなく、必要に応じて辿れる段階的開示の構造です。
- **State** — 何を終えたか、いま何をしているか、次に何をするかを追跡します。ディスクに永続化されるため、次のセッションは前回の続きから正確に始められます。
- **Verification** — 合格したテストスイートだけが証拠になります。実行可能な裏付けなしに、エージェントは勝利を宣言できません。
- **Scope** — エージェントを一度に 1 つの機能に制限します。過剰拡張なし。3 つのことを中途半端に終わらせない。未完了の作業を隠すために機能一覧を書き換えない。
- **Session Lifecycle** — 開始時に初期化し、最後に片付けます。次のセッションがすぐ再開できる、クリーンな状態を残します。

---

## この講座が存在する理由

問いは「モデルはコードを書けるか？」ではありません。書けます。本当に問うべきなのは、**複数セッションにまたがって、実際のリポジトリ内で、人の継続的な監督なしに、現実のエンジニアリング作業を安定して完了できるか？** です。

現時点の答えは、ハーネスなしでは無理、です。

```text
    ハーネスなし                         ハーネスあり
    ============                         ============

    セッション 1: agent がコードを書く      セッション 1: agent が指示を読む
              agent がテストを壊す                  agent が init.sh を実行する
              agent が「完了」と言う               agent が 1 つの機能だけに取り組む
              あなたが手で直す                      agent が完了を宣言する前に検証する
                                                      agent が進捗ログを更新する
    セッション 2: agent が新しく始める            agent がクリーンな状態をコミットする
              agent には前回の記憶がない          セッション 2: agent が進捗ログを読む
              何が起きたか分からない               agent が前回の続きから正確に再開する
              agent が同じ作業をやり直す            agent が未完了の機能を続ける
              あるいは別のことを始める            あなたは救済ではなくレビューをする
              あなたがまた直す

    結果: あなたは                    結果: agent が仕事をし、
          自分でやるより多く                あなたは結果を確認する
          後始末に時間を使う
```

この講座が本当に扱う問いは次のとおりです。

- どの harness 設計がタスク完了率を改善するのか？
- どの設計が手戻りや誤完了を減らすのか？
- どの仕組みが長時間タスクを安定して進め続けるのか？
- どの構造が、エージェントを何度動かしても保守しやすいシステムを維持するのか？

---

## コースカリキュラムとドキュメント

講座全体の教材は、**[ドキュメントサイト](https://walkinglabs.github.io/learn-harness-engineering/)** で確認してください。

カリキュラムは 3 つのパートに分かれています。

1. **Lectures**: harness engineering の理論を説明する 12 の概念単元
2. **Projects**: agentic workspace をゼロから構築する 6 つのハンズオンプロジェクト
3. **Resource Library**: 自分のリポジトリですぐ使える、コピペ可能なテンプレート（`AGENTS.md`、`feature_list.json`、`init.sh` など）

---

<a id="quick-start-improve-your-agent-today"></a>
## クイックスタート: 今すぐエージェントを改善する

価値を得るために、12 講義すべてを最初から読む必要はありません。すでに実案件でコーディングエージェントを使っているなら、今すぐ改善する方法があります。

考え方は単純です。プロンプトだけを書くのではなく、エージェントに「何をするか」「何が終わったか」「どう検証するか」を定義した構造化ファイル群を与えます。これらのファイルはリポジトリ内に置かれるため、毎回のセッションが同じ状態から始まります。

```text
    YOUR PROJECT ROOT
    ├── AGENTS.md              <-- エージェントの操作マニュアル
    ├── CLAUDE.md              <-- (Claude Code を使う場合の代替)
    ├── init.sh                <-- install + verify + start を実行
    ├── feature_list.json      <-- どんな機能があり、何が完了したか
    ├── claude-progress.md     <-- 各セッションで何が起きたか
    └── src/                   <-- 実際のコード
```

[Resource Library](https://walkinglabs.github.io/learn-harness-engineering/en/resources/) からスターターテンプレートを取り出して、自分のプロジェクトに入れてください。それだけです。たった 4 ファイルで、エージェントのセッションはプロンプトだけで動かすよりずっと安定します。

---

<a id="capstone-project-a-real-app"></a>
## キャップストーンプロジェクト: 実アプリ

6 つのコースプロジェクトはすべて、同じ製品を中心に進みます。**Electron ベースの個人用ナレッジベースデスクトップアプリ** です。

```text
    ┌─────────────────────────────────────────────────────┐
    │               ナレッジベースデスクトップアプリ        │
    │                                                     │
    │  ┌──────────────┐  ┌──────────────────────────────┐│
    │  │ Document List │  │       Q&A Panel              ││
    │  │              │  │                              ││
    │  │ doc-001.md   │  │  Q: harness eng とは？       ││
    │  │ doc-002.md   │  │  A: エージェントモデルの周囲 ││
    │  │ doc-003.md   │  │     に構築された環境...       ││
    │  │ ...          │  │     [citation: doc-002.md]   ││
    │  └──────────────┘  └──────────────────────────────┘│
    │                                                     │
    │  ┌─────────────────────────────────────────────────┐│
    │  │ Status Bar: 42 docs | 38 indexed | last sync 3m ││
    │  └─────────────────────────────────────────────────┘│
    └─────────────────────────────────────────────────────┘

    コア機能:
    ├── ローカル文書を取り込む
    ├── 文書ライブラリを管理する
    ├── 文書を処理してインデックス化する
    ├── 取り込んだ内容に対して AI 搭載 Q&A を実行する
    └── 根拠付きの回答と引用を返す
```

このプロジェクトが選ばれたのは、実用性が高く、現実的な製品としての複雑さがあり、ハーネス改善の前後差を観察するのに適しているからです。

各コースプロジェクトの starter/solution は、その進化段階にあるこの Electron アプリの完全なコピーです。P(N+1) の starter は P(N) の solution から派生します。つまり、ハーネスの技術が上がるにつれて、アプリも進化していくわけです。

---

<a id="learning-path"></a>
## 学習パス

この講座は順番どおりに進めることを前提に設計されています。各フェーズは前の内容の上に積み上がります。

```text
    Phase 1: 問題を見つける           Phase 2: リポジトリを構造化する
    ========================           ==========================

    L01  強いモデル ≠ 信頼できる        L03  リポジトリを唯一の
         実行                                真実の源にする
    L02  harness とは実際に何か
                                       L04  指示を 1 つの巨大ファイルでは
         |                                   なく分割する
         v
    P01  プロンプトのみ vs.               |
         rules-first の比較               v
                                               P02  エージェントが読める workspace


    Phase 3: セッションをつなぐ         Phase 4: フィードバックとスコープ
    ==========================           =========================

    L05  セッションをまたいでも          L07  タスク境界を明確に引く
         コンテキストを維持する
                                       L08  harness の原始要素としての
    L06  すべてのエージェント                 機能一覧
         セッション前に初期化する
                                               |
         |                                     v
         v                                     P04  エージェントの振る舞いを
    P03  複数セッションの継続性                  修正するためのランタイムフィードバック


    Phase 5: 検証                       Phase 6: すべてを組み合わせる
    =====================               ============================

    L09  エージェントが早く                L11  エージェントの実行時
         勝利宣言しないようにする              の挙動を可視化する

    L10  フルパイプライン実行 =          L12  各セッションの最後に
         本当の検証                         クリーンな引き継ぎを行う

         |                                     |
         v                                     v
    P05  エージェント自身に作業を検証させる   P06  完全なハーネスを構築する
                                               (capstone project)
```

各フェーズは、パートタイムで進めるならおよそ 1 週間です。もっと速く進めたいなら、フェーズ 1〜3 は長い週末で終えられます。

---

<a id="syllabus"></a>
## シラバス

### Lectures — 12 の概念単元、それぞれが 1 つの核心的な問いに答える

*各講義の全文は [ドキュメントサイト](https://walkinglabs.github.io/learn-harness-engineering/) で読んでください。*

| セッション | 問い | 中核となる考え |
|---------|------|----------------|
| [L01](./docs/lectures/lecture-01-why-capable-agents-still-fail/index.md) | なぜ強いモデルでも実タスクで失敗するのか？ | ベンチマークと現実のエンジニアリングの間にある能力ギャップ |
| [L02](./docs/lectures/lecture-02-what-a-harness-actually-is/index.md) | 「harness」とは実際には何を意味するのか？ | 5 つのサブシステム: instructions、state、verification、scope、lifecycle |
| [L03](./docs/lectures/lecture-03-why-the-repository-must-become-the-system-of-record/index.md) | なぜリポジトリが唯一の真実の源でなければならないのか？ | エージェントに見えないものは存在しない |
| [L04](./docs/lectures/lecture-04-why-one-giant-instruction-file-fails/index.md) | なぜ巨大な 1 つの指示ファイルは失敗するのか？ | 段階的開示: 百科事典ではなく地図を与える |
| [L05](./docs/lectures/lecture-05-why-long-running-tasks-lose-continuity/index.md) | なぜ長時間タスクは継続性を失うのか？ | 進捗をディスクに保存し、前回の続きから再開する |
| [L06](./docs/lectures/lecture-06-why-initialization-needs-its-own-phase/index.md) | なぜ初期化には専用フェーズが必要なのか？ | エージェントが作業を始める前に、環境が健全か確認する |
| [L07](./docs/lectures/lecture-07-why-agents-overreach-and-under-finish/index.md) | なぜエージェントはやり過ぎ、終わり切れないのか？ | 1 つずつ機能を進める。完了条件を明示する |
| [L08](./docs/lectures/lecture-08-why-feature-lists-are-harness-primitives/index.md) | なぜ機能一覧は harness の原始要素なのか？ | エージェントが無視できない、機械可読なスコープ境界 |
| [L09](./docs/lectures/lecture-09-why-agents-declare-victory-too-early/index.md) | なぜエージェントは勝利を早く宣言しすぎるのか？ | 検証の穴: 自信 ≠ 正しさ |
| [L10](./docs/lectures/lecture-10-why-end-to-end-testing-changes-results/index.md) | なぜ end-to-end テストで結果が変わるのか？ | フルパイプライン実行だけが本当の検証になる |
| [L11](./docs/lectures/lecture-11-why-observability-belongs-inside-the-harness/index.md) | なぜ可観測性は harness の内側にあるべきなのか？ | エージェントが何をしたか見えなければ、壊したものも直せない |
| [L12](./docs/lectures/lecture-12-why-every-session-must-leave-a-clean-state/index.md) | なぜ各セッションはクリーンな状態で終わる必要があるのか？ | 次のセッションの成功は、このセッションの片付けに依存する |

### Projects — 同じ Electron アプリに講義の方法を適用する 6 つのハンズオンプロジェクト

| Project | やること | Harness の仕組み |
|---------|----------|-------------------|
| [P01](./docs/projects/project-01-baseline-vs-minimal-harness/index.md) | 同じタスクを 2 回実行する: prompt-only と rules-first | 最小限の harness: `AGENTS.md` + `init.sh` + `feature_list.json` |
| [P02](./docs/projects/project-02-agent-readable-workspace/index.md) | エージェントが読めるようにリポジトリを再構成する | エージェントが読める workspace + 永続的な state ファイル |
| [P03](./docs/projects/project-03-multi-session-continuity/index.md) | エージェントが前回の続きから再開できるようにする | 進捗ログ + セッションの引き継ぎ + 複数セッション継続性 |
| [P04](./docs/projects/project-04-incremental-indexing/index.md) | エージェントがやり過ぎたり、やらなさ過ぎたりするのを防ぐ | ランタイムフィードバック + スコープ制御 + 増分インデックス化 |
| [P05](./docs/projects/project-05-grounded-qa-verification/index.md) | エージェント自身に作業を検証させる | self-verification + 根拠付き Q&A + 証拠ベースの完了判定 |
| [P06](./docs/projects/project-06-runtime-observability-and-debugging/index.md) | ゼロから完全な harness を構築する (capstone) | 完全な harness: すべての仕組み + 可観測性 + ablation study |

```text
    プロジェクトの進化
    ==================

    P01  prompt-only vs. rules-first      問題が見える
     |
     v
    P02  エージェントが読める workspace   リポジトリを再構成する
     |
     v
    P03  複数セッションの継続性         セッションをつなぐ
     |
     v
    P04  ランタイムフィードバック & スコープ  フィードバックループを加える
     |
     v
    P05  self-verification               エージェントに自分をチェックさせる
     |
     v
    P06  完全な harness (capstone)       完全なシステムを構築する

    各プロジェクトの solution が、次のプロジェクトの starter になる。
    アプリは進化し、あなたの harness 技術もそれに合わせて成長する。
```

### Resource Library

- [English Resource Library](https://walkinglabs.github.io/learn-harness-engineering/en/resources/) — テンプレート、チェックリスト、方法の参照
- [Chinese Resource Library](https://walkinglabs.github.io/learn-harness-engineering/zh/resources/) — 中文模板、清单和方法参考
- [Russian Resource Library](https://walkinglabs.github.io/learn-harness-engineering/ru/resources/) — テンプレート、チェックリスト、参照資料
- [Vietnamese Resource Library](https://walkinglabs.github.io/learn-harness-engineering/vi/resources/) — テンプレート、チェックリスト、参照資料

---

## Agent セッションのライフサイクル

この講座の中核となる考え方の 1 つは、**エージェントのセッションは無秩序ではなく、構造化されたライフサイクルに従うべきだ** ということです。流れは次のとおりです。

```text
    エージェントのセッションライフサイクル
    ==================================

    ┌──────────────────────────────────────────────────────────────────┐
    │  開始                                                            │
    │                                                                  │
    │  1. Agent が AGENTS.md / CLAUDE.md を読む                       │
    │  2. Agent が init.sh を実行する (install, verify, health check) │
    │  3. Agent が claude-progress.md を読む (前回何が起きたか)        │
    │  4. Agent が feature_list.json を読む (完了済み、次にやること)   │
    │  5. Agent が git log を確認する (最近の変更)                    │
    │                                                                  │
    │  選択                                                            │
    │                                                                  │
    │  6. Agent が未完了の機能を 1 つだけ選ぶ                         │
    │  7. Agent はその機能だけに取り組む                              │
    │                                                                  │
    │  実行                                                            │
    │                                                                  │
    │  8. Agent が機能を実装する                                       │
    │  9. Agent が検証を実行する (tests, lint, type-check)            │
    │  10. 検証に失敗したら: 修正して再実行する                       │
    │  11. 検証に通ったら: 証拠を記録する                              │
    │                                                                  │
    │  まとめ                                                          │
    │                                                                  │
    │  12. Agent が claude-progress.md を更新する                     │
    │  13. Agent が feature_list.json を更新する                      │
    │  14. Agent がまだ壊れているもの、未検証のものを記録する         │
    │  15. Agent が commit する (安全に再開できるときだけ)           │
    │  16. Agent が次のセッションに向けたクリーンな再開経路を残す    │
    │                                                                  │
    └──────────────────────────────────────────────────────────────────┘

    harness はこのライフサイクルのすべての遷移を統制する。
    モデルは各ステップでどのコードを書くかを決める。
    harness がなければ、ステップ 9 は「agent が問題なさそうと言う」になる。
    harness があれば、ステップ 9 は「テスト合格、lint はクリーン、型チェックも通過」になる。
```

---

## この講座の対象者

この講座は、次のような人向けです。

- すでにコーディングエージェントを使っていて、安定性と品質をさらに高めたいエンジニア
- harness 設計を体系的に理解したい研究者やビルダー
- 環境設計がエージェント性能にどう影響するかを知りたいテックリード

この講座は、次のような人向けではありません。

- ノーコードの AI 入門を探している人
- プロンプトだけに関心があり、実装までやるつもりがない人
- 実際のリポジトリ内でエージェントに作業させる準備がない学習者

---

## 必要条件

これは、実際にコーディングエージェントを動かす講座です。

少なくとも次のいずれか 1 つが必要です。

- Claude Code
- Codex
- ファイル編集、コマンド実行、マルチステップタスクに対応した別の IDE または CLI のコーディングエージェント

この講座は、次のことができる前提です。

- ローカルリポジトリを開ける
- エージェントにファイル編集を許可できる
- エージェントにコマンド実行を許可できる
- 出力を確認し、タスクを再実行できる

そのようなツールがない場合でも、講座内容を読むことはできますが、意図どおりにプロジェクトを完了することはできません。

---

## ローカルプレビュー

このリポジトリは、ドキュメントビューアとして VitePress を使っています。

```sh
npm install
npm run docs:dev        # ホットリロード付きの開発サーバー
npm run docs:build      # 本番ビルド
npm run docs:preview    # ビルド済みサイトのプレビュー
```

そのあと、VitePress が出力するローカル URL をブラウザで開いてください。

---

## 前提条件

必須:

- ターミナル、git、ローカル開発環境に関する基本的な知識
- 少なくとも 1 つの一般的なアプリケーションスタックでコードを読み書きできること
- 基本的なソフトウェアデバッグ経験（ログ、テスト、実行時の挙動を読む力）
- 実装重視の講座に取り組めるだけの時間

あると助かるが必須ではない:

- Electron、デスクトップアプリ、local-first ツールの経験
- テスト、ログ、ソフトウェアアーキテクチャの知識
- Codex、Claude Code、または類似のコーディングエージェントを使った経験

---

## 主要参考資料

主要:

- [OpenAI: Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/)
- [Anthropic: Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
- [Anthropic: Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps)
- [OpenAI: Unrolling the Codex agent loop](https://openai.com/index/unrolling-the-codex-agent-loop/)
- [Anthropic: Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)
- [LangChain: Improving Deep Agents with harness engineering](https://www.langchain.com/blog/improving-deep-agents-with-harness-engineering)
- [Thoughtworks / Martin Fowler: Harness engineering for coding agent users](https://martinfowler.com/articles/harness-engineering.html)
- [Cursor: Continually improving our agent harness](https://cursor.com/blog/continually-improving-agent-harness)

詳細な階層構造の参考一覧は [`docs/en/resources/reference/`](./docs/en/resources/reference/index.md) を参照してください。

---

## リポジトリ構成

```text
learn-harness-engineering/
├── docs/                          # VitePress のドキュメントサイト
│   ├── lectures/                  # 12 講義 (index.md + code/ examples)
│   │   ├── lecture-01-*/
│   │   ├── lecture-02-*/
│   │   └── ... (全 12)
│   ├── projects/                  # 6 つのプロジェクト説明
│   │   ├── project-01-*/
│   │   └── ... (全 6)
│   └── resources/                 # 多言語テンプレートと参照資料
│       ├── en/                    # 英語のテンプレート、チェックリスト、ガイド
│       ├── zh/                    # 中国語のテンプレート、チェックリスト、ガイド
│       ├── ru/                    # ロシア語のテンプレート、チェックリスト、ガイド
│       └── vi/                    # ベトナム語のテンプレート、チェックリスト、ガイド
├── projects/
│   ├── shared/                    # 共有の Electron + TypeScript + React 基盤
│   └── project-NN/                # 各プロジェクトの starter/ と solution/ ディレクトリ
├── skills/                        # 再利用可能な AI エージェント skills
│   └── harness-creator/           # Harness engineering skill
├── package.json                   # VitePress + 開発ツール
└── CLAUDE.md                      # このリポジトリ向けの Claude Code 指示
```

---

## 講座の構成

- 各講義は 1 つの問いに集中します
- 講座には 6 つのプロジェクトがあります
- すべてのプロジェクトで、エージェントに実際の作業をさせます
- すべてのプロジェクトで、弱い harness と強い harness の結果を比較します
- 重要なのは、書いたドキュメントの量ではなく、測定された差分です

---

## Skills

このリポジトリには、IDE やエージェント workspace に直接インストールして再利用できる AI エージェント skills も含まれています。

- [**harness-creator**](./skills/harness-creator/): 自分のプロジェクト向けに本番グレードの harness を数分で組み立てるのを助ける skill です。

---

<a id="other-courses"></a>
## 他の講座

私たちのチームは他にも講座を公開しています。ぜひ見てください。

[![Hands-on Modern RL](https://img.shields.io/badge/HANDS--ON_MODERN_RL-0052cc?style=for-the-badge)](https://github.com/walkinglabs/hands-on-modern-rl)

**Hands-on Modern RL**: 基礎的な RL の概念から、LLM アライメント、RLVR、そして高度な Agentic システムへとつなぐ、オープンソースの実践的カリキュラムです。

---

## スター履歴

[![Star History Chart](https://api.star-history.com/svg?repos=walkinglabs/learn-harness-engineering&type=date&legend=top-left)](https://www.star-history.com/#walkinglabs/learn-harness-engineering&type=date&legend=top-left)

---

## 謝辞

この講座は、[learn-claude-code](https://github.com/shareAI-lab/learn-claude-code) に触発され、そのアイデアを取り入れています。これは、単一ループから分離された自律実行まで、エージェントをゼロから構築するための段階的ガイドです。
