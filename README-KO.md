[英語](https://walkinglabs.github.io/learn-harness-engineering/en/) · [中国語版](https://walkinglabs.github.io/learn-harness-engineering/zh/) · [ロシア語版](https://walkinglabs.github.io/learn-harness-engineering/ru/) · [ベトナム語版](https://walkinglabs.github.io/learn-harness-engineering/vi/) · **日本語**

# Learn Harness Engineering

> **AI コーディングエージェント(coding agent)が安定して動くために必要な、環境、状態管理、検証(verification)、制御メカニズムを構築するためのプロジェクトベース講義です。**

Learn Harness Engineering は、AI コーディングエージェント工学に特化した講義です。業界最先端のハーネスエンジニアリング(Harness Engineering)の理論と実践を深く掘り下げ、体系的に整理しています。主な参考資料は次のとおりです。

- [OpenAI: Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/)
- [Anthropic: Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
- [Anthropic: Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps)
- [Awesome Harness Engineering](https://github.com/walkinglabs/awesome-harness-engineering)

> **すぐに始めたいですか?** [`skills/harness-creator/`](./skills/) スキル(skill)を使えば、数分で本格的なハーネス(AGENTS.md, feature list, init.sh, 検証ワークフロー)をプロジェクトに直接スキャフォールド(scaffold)できます。

---

## ビジュアルプレビュー

### 講義ホームページ
> 中核となる考え方を紹介し、始め方を明確に示す総合的な講義の概要です。

![講義ホームページのプレビュー](./docs/public/screenshots/readme/en-home.png)

### 没入型の講義
> 実際の問題状況を深く掘り下げ、実習プロジェクト(例: Project 01)を通じて没入感のある学習体験を提供します。

![講義のプレビュー](./docs/public/screenshots/readme/en-lecture-01.png)

### すぐに使えるリソースライブラリ
> マルチターンの AI エージェント開発でよく起きるコンテキスト(context)の喪失や、早すぎる完了宣言などの問題を解決するために設計された、テンプレートと参考設定の集まりです。

![リソースライブラリのプレビュー](./docs/public/screenshots/readme/en-resources.png)

## PDF 講義資料

このリポジトリには、講義コンテンツ向けの PDF ビルドパイプラインが含まれています。

- `npm run pdf:build` コマンドで、英語・中国語の PDF をローカル生成できます。
- 出力ファイルは `artifacts/pdfs/` に保存されます。
- README のプレビュー画像を更新するには `npm run screenshots:readme` を実行してください。
- GitHub Actions ワークフロー [`release-course-pdfs.yml`](./.github/workflows/release-course-pdfs.yml) を使って PDF をビルドし、GitHub Releases に公開できます。

---

## モデルは賢く、安定性はハーネスが生む

多くの人は、自分で経験して初めて気づく厳しい現実があります。**世界で最も強力なモデルでも、適切な環境がなければ実際のエンジニアリング作業では失敗します。**

おそらく、似た経験をしたことがあるはずです。Claude や GPT にリポジトリ(repository)での作業を任せると、最初は順調に見えます。ファイルを読み、コードを書き、生産的に見えます。ところが、どこかで崩れます。手順を飛ばし、テストを壊し、「完了」と言うのに実際には何も動かない。自分でやっていればもっと早かったはずの時間を、立て直しに費やしてしまいます。

これはモデルの問題ではありません。ハーネスの問題です。

証拠は明確です。Anthropic は制御された実験を行いました。モデルは同じ(Opus 4.5)、プロンプトも同じ("2D レトロゲームエディタを作る")でした。ハーネスなしでは、20分で $9 を使っても、動かない成果物しか得られませんでした。完全なハーネス(プランナー + ジェネレーター + 評価器)を使うと、6時間で $200 をかけて、実際に遊べるゲームを作れました。モデルは変わっていません。変わったのはハーネスです。

OpenAI も Codex で同じことを報告しています。よく整えられたリポジトリでは、同じモデルが「不安定」から「安定」に変わります。これは小さな改善ではなく、質的な変化です。

**この講義は、その環境を構築する方法を教えます。**

```text
                    ハーネス・パターン
                    ====================

    あなた --> タスクを渡す --> エージェントがハーネスファイルを読む --> エージェントが実行する
                                                                    |
                                                          ハーネスがすべての手順を統制する:
                                                          |
                                                          +--> Instructions: 何を、どの順番でやるか
                                                          +--> Scope:         一度に1機能だけに絞る
                                                          +--> State:         進捗ログ、機能一覧、git 履歴
                                                          +--> Verification:  tests, lint, type-check, smoke run
                                                          +--> Lifecycle:      最初に init、最後にクリーンな状態
                                                          |
                                                          v
                                                     検証に通るまで
                                                     エージェントは止まらない
```

---

## ハーネスエンジニアリングとは何か

ハーネスエンジニアリング(Harness Engineering)とは、モデルが安定した結果を出せるように、モデルの周囲に完全な作業環境を構築することです。より良いプロンプトを書くことではありません。モデルが確実に動くシステムを設計することです。

ハーネスには、5つのサブシステムがあります。

```text
    ┌─────────────────────────────────────────────────────────────────┐
    │                        ハーネス                                 │
    │                                                                 │
    │   ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐ │
    │   │ Instructions │  │    State     │  │   Verification       │ │
    │   │              │  │              │  │                      │ │
    │   │ AGENTS.md    │  │ progress.md  │  │ tests + lint         │ │
    │   │ CLAUDE.md    │  │ feature_list │  │ type-check           │ │
    │   │ feature_list │  │ git log      │  │ smoke runs           │ │
    │   │ docs/        │  │ session hand │  │ e2e pipeline         │ │
    │   └──────────────┘  └──────────────┘  └──────────────────────┘ │
    │                                                                 │
    │   ┌──────────────┐  ┌──────────────────────────────────────┐   │
    │   │    Scope     │  │        Session Lifecycle             │   │
    │   │              │  │                                      │   │
    │   │ 1回に1機能    │  │ start 時に init.sh                 │   │
    │   │ だけにする   │  │ end 時に clean-state checklist     │   │
    │   │ 完了定義     │  │ 次回セッションへの handoff note    │   │
    │   │              │  │ 安全に再開できる時だけ commit      │   │
    │   └──────────────┘  └──────────────────────────────────────┘   │
    │                                                                 │
    └─────────────────────────────────────────────────────────────────┘

    MODEL が書くコードを決める。
    HARNESS は、それをいつ、どこに、どう書くかを統制する。
    ハーネスはモデルを賢くするのではない。
    モデルの出力を信頼できるものにする。
```

各サブシステムには、それぞれ1つの役割があります。

- **Instructions** — エージェントに何をどの順番でやるか、開始前に何を読むべきかを伝えます。1つの巨大なファイルではなく、必要に応じて探索する progressive disclosure の構造です。
- **State** — 完了したもの、進行中のもの、次の作業を追跡します。ディスクに永続化(persist)されるため、次のセッションは前回終了時の正確な位置から引き継げます。
- **Verification** — 通過したテストスイートだけが証拠として認められます。実行可能な証拠なしに、エージェントは完了を宣言できません。
- **Scope** — エージェントを一度に1機能に制限します。やり過ぎ(overreach)はありません。3つを半分ずつ完成させることもありません。未完了の作業を隠すために機能一覧を書き換えることもありません。
- **Session Lifecycle** — 開始時に初期化し、終了時に整理します。次のセッションのために、きれいな再開経路を残します。

---

## この講義が存在する理由

問いは「モデルはコードを書けるのか?」ではありません。モデルは書けます。問いはこれです。**モデルは、実際のリポジトリで、複数セッションにわたって継続的な人間の監督なしに、実際のエンジニアリング作業を安定してやり切れるのか?**

現時点での答えはこうです。ハーネスがなければ、できません。

```text
    ハーネスなし                           ハーネスあり
    ==========                            ==========

    Session 1: エージェントがコードを書く    Session 1: エージェントが説明を読む
              テストを壊す                           init.sh を実行する
              「done」と言う                         1つの機能だけに取り組む
              あなたが手で直す                       完了と主張する前に検証する
                                                   進捗ログを更新する
    Session 2: エージェントが新規開始          クリーンな状態で commit する
              以前の出来事を覚えていない
              作業をやり直す               Session 2: エージェントが進捗ログを読む
              あるいは別のことをする               ちょうど中断したところから再開する
              あなたがまた直す                     未完了の機能を続ける
                                                    あなたは救済ではなくレビューする

    結果: 自分でやるより                結果: エージェントが作業し、
          片付けに時間を使う                    あなたは結果を検証する
```

この講義で実際に扱う問いは次のとおりです。

- どのハーネス設計が作業完了率を改善するのか?
- どの設計が手戻りと誤った完了宣言を減らすのか?
- どのメカニズムが長時間の作業を着実に進め続けるのか?
- どの構造なら、複数回のエージェント実行を経ても保守可能なシステムを保てるのか?

---

## 講義カリキュラムと文書

講義資料全体は **[ドキュメントサイト](https://walkinglabs.github.io/learn-harness-engineering/)** で確認してください。

カリキュラムは3つの部分に分かれています。

1. **講義**: ハーネスエンジニアリングの理論を説明する12の概念単元。
2. **プロジェクト**: ゼロからエージェント用ワークスペースを構築する6つの実習プロジェクト。
3. **リソースライブラリ**: 今すぐ自分のリポジトリで使える、コピペ準備済みのテンプレート(`AGENTS.md`, `feature_list.json`, `init.sh` など)。

---

## クイックスタート: 今すぐエージェントを改善する

価値を得るために、12の講義をすべて読む必要はありません。すでに実際のプロジェクトでコーディングエージェントを使っているなら、今すぐ改善する方法があります。

考え方はシンプルです。プロンプトだけを書く代わりに、エージェントに、何をすべきか、何が完了したか、作業をどう検証するかを定義した構造化ファイルのセットを与えてください。これらのファイルはリポジトリ内に置かれるため、すべてのセッションが同じ状態から始まります。

```text
    プロジェクトルート
    ├── AGENTS.md              <-- エージェントの操作マニュアル
    ├── CLAUDE.md              <-- (Claude Code を使う場合の代替)
    ├── init.sh                <-- install + verify + start を実行
    ├── feature_list.json      <-- どんな機能があり、何が完了しているか
    ├── claude-progress.md     <-- 各セッションで何が起きたか
    └── src/                   <-- 実際のコード
```

[リソースライブラリ](https://walkinglabs.github.io/learn-harness-engineering/en/resources/) から開始テンプレートを取り出して、プロジェクトに追加してください。たった4つのファイルでも、プロンプトだけで動かすよりはるかに安定したエージェントセッションになります。

---

## キャップストーンプロジェクト: 実アプリ

6つの講義プロジェクトはすべて、同じ製品を中心に展開します。**Electron ベースの個人用ナレッジベース(knowledge base)デスクトップアプリ**です。

```text
    ┌─────────────────────────────────────────────────────┐
    │               ナレッジベース デスクトップアプリ   │
    │                                                     │
    │  ┌──────────────┐  ┌──────────────────────────────┐│
    │  │ ドキュメント一覧 │  │       Q&A パネル          ││
    │  │              │  │                              ││
    │  │ doc-001.md   │  │  Q: harness eng とは?      ││
    │  │ doc-002.md   │  │  A: エージェントモデルを    ││
    │  │ doc-003.md   │  │     取り巻く環境...         ││
    │  │ ...          │  │     [citation: doc-002.md] ││
    │  └──────────────┘  └──────────────────────────────┘│
    │                                                     │
    │  ┌─────────────────────────────────────────────────┐│
    │  │ ステータスバー: 42 docs | 38 indexed | last sync 3m ││
    │  └─────────────────────────────────────────────────┘│
    └─────────────────────────────────────────────────────┘

    コア機能:
    ├── ローカル文書を取り込む
    ├── 文書ライブラリを管理する
    ├── 文書を処理して索引化する
    ├── 取り込んだ内容に対して AI 搭載の Q&A を実行する
    └── 根拠付きの回答を引用とともに返す
```

このプロジェクトが選ばれたのは、強い実用価値があり、実際の製品として十分な複雑さがあり、ハーネス改善の前後を観察するのに適した環境だからです。

各講義プロジェクトのスターター/ソリューションは、その進化段階におけるこの Electron アプリの完全なコピーです。P(N+1) のスターターは P(N) のソリューションから派生します。アプリは、ハーネスの技術が成長するのに合わせて進化していきます。

---

## 学習パス

講義は順番に進めるよう設計されています。各段階は前の段階の上に積み上がります。

```text
    Phase 1: 問題を見る                 Phase 2: リポジトリを構造化する
    ========================             ==========================

    L01  強いモデル != 信頼できる        L03  リポジトリを
         実行                           単一の真実源にする
    L02  ハーネスの実際の意味
                                     L04  1つの巨大なファイルではなく
         |                               複数ファイルに指示を分割する
         v
    P01  プロンプトだけ vs.              |
         ルール先行の比較                v
                                       P02  エージェントが読めるワークスペース


    Phase 3: セッションをつなぐ       Phase 4: フィードバック & スコープ
    ==========================         =========================

    L05  セッションをまたいで         L07  明確なタスク境界を引く
         コンテキストを保つ
                                     L08  ハーネスの基本部品としての
    L06  すべてのエージェント            機能一覧
         セッションの前に初期化する
                                             |
         |                                   v
         v                                   P04  実行時フィードバックで
    P03  マルチセッションの継続性        正しいエージェント挙動へ


    Phase 5: 検証                     Phase 6: すべてをまとめる
    =====================             ============================

    L09  エージェントが               L11  エージェントの実行時状態を
         早く勝利宣言するのを防ぐ          可視化可能にする

    L10  フルパイプライン実行 =       L12  毎セッション終了時に
         本当の検証                       クリーンに引き渡す

         |                               |
         v                               v
    P05  エージェント自身が           P06  完全なハーネスを構築する
         自分の作業を検証する               (キャップストーンプロジェクト)
```

パートタイムで進めるなら、各段階におよそ1週間かかります。もっと速く進めるなら、1〜3段階は長い週末で終えられます。

---

## シラバス

### 講義 - 12の概念単元、それぞれが1つの核心的な問いに答える

*各講義の全文は [ドキュメントサイト](https://walkinglabs.github.io/learn-harness-engineering/) で読めます。*

| セッション | 質問 | 核心アイデア |
|------|------|--------------|
| [L01](./docs/lectures/lecture-01-why-capable-agents-still-fail/index.md) | なぜ強力なモデルは、実際の作業でそれでも失敗するのか? | ベンチマークと実際のエンジニアリングのあいだにある能力ギャップ |
| [L02](./docs/lectures/lecture-02-what-a-harness-actually-is/index.md) | "ハーネス" とは実際には何を意味するのか? | 5つのサブシステム: Instructions, State, Verification, Scope, Lifecycle |
| [L03](./docs/lectures/lecture-03-why-the-repository-must-become-the-system-of-record/index.md) | なぜリポジトリが Single Source of Truth でなければならないのか? | エージェントに見えなければ、存在しないのと同じ |
| [L04](./docs/lectures/lecture-04-why-one-giant-instruction-file-fails/index.md) | なぜ1つの巨大な指示ファイルは失敗するのか? | progressive disclosure: 百科事典ではなく地図を渡せ |
| [L05](./docs/lectures/lecture-05-why-long-running-tasks-lose-continuity/index.md) | なぜ長時間タスクは連続性(continuity)を失うのか? | 進捗をディスクに永続化し、中断した場所から再開する |
| [L06](./docs/lectures/lecture-06-why-initialization-needs-its-own-phase/index.md) | なぜ初期化には独立した段階が必要なのか? | エージェントが作業を始める前に、環境が正常であることを検証する |
| [L07](./docs/lectures/lecture-07-why-agents-overreach-and-under-finish/index.md) | なぜエージェントは範囲を超え、未完成で終わるのか? | 1回に1機能; 完了の明示的定義 |
| [L08](./docs/lectures/lecture-08-why-feature-lists-are-harness-primitives/index.md) | なぜ機能一覧はハーネスの基本構成要素なのか? | エージェントが無視できない、機械可読なスコープ境界 |
| [L09](./docs/lectures/lecture-09-why-agents-declare-victory-too-early/index.md) | なぜエージェントは早すぎる勝利宣言をするのか? | 検証ギャップ: 自信 ≠ 正確さ |
| [L10](./docs/lectures/lecture-10-why-end-to-end-testing-changes-results/index.md) | なぜエンドツーエンドテストは結果を変えるのか? | フルパイプライン実行だけが実際の検証として認められる |
| [L11](./docs/lectures/lecture-11-why-observability-belongs-inside-the-harness/index.md) | なぜ可観測性(observability)はハーネスの内部にあるべきなのか? | エージェントが何をしたか見えなければ、何が悪いか直せない |
| [L12](./docs/lectures/lecture-12-why-every-session-must-leave-a-clean-state/index.md) | なぜすべてのセッションはクリーンな状態を残す必要があるのか? | 次のセッションの成功は、このセッションの整理にかかっている |

### プロジェクト - 同じ Electron アプリに講義の方法を適用する6つの実習プロジェクト

| プロジェクト | 内容 | ハーネスメカニズム |
|---------|----------|----------------|
| [P01](./docs/projects/project-01-baseline-vs-minimal-harness/index.md) | 同じ作業を2回実行: プロンプトのみ vs. ルール先行 | 最小ハーネス: AGENTS.md + init.sh + feature_list.json |
| [P02](./docs/projects/project-02-agent-readable-workspace/index.md) | エージェントが読めるようにリポジトリを再構成する | エージェント可読ワークスペース + 永続状態ファイル |
| [P03](./docs/projects/project-03-multi-session-continuity/index.md) | エージェントが中断地点から引き継げるようにする | 進捗ログ + セッション handoff + マルチセッション継続性 |
| [P04](./docs/projects/project-04-incremental-indexing/index.md) | エージェントがやり過ぎたり少なすぎたりしないようにする | 実行時フィードバック + スコープ制御 + 増分インデクシング |
| [P05](./docs/projects/project-05-grounded-qa-verification/index.md) | エージェントに自分の作業を検証させる | 自己検証 + 根拠ベースの Q&A + 証拠ベースの完了 |
| [P06](./docs/projects/project-06-runtime-observability-and-debugging/index.md) | 最初から完全なハーネスを構築する (キャップストーンプロジェクト) | 完全なハーネス: すべてのメカニズム + 可観測性 + 制御された研究 |

```text
    プロジェクトの進化
    =================

    P01  プロンプトのみ vs. ルール先行   問題が見える
     |
     v
    P02  エージェント可読ワークスペース   リポジトリを再構成する
     |
     v
    P03  マルチセッションの継続性        セッションをつなぐ
     |
     v
    P04  実行時フィードバック & スコープ   フィードバックループを追加する
     |
     v
    P05  自己検証                        エージェント自身に確認させる
     |
     v
    P06  完全なハーネス (キャップストーン) フルシステムを構築する

    各プロジェクトのソリューションが、次のプロジェクトのスターターになります。
    アプリは進化し、それに合わせてあなたのハーネススキルも成長します。
```

### リソースライブラリ

- [韓国語リソースライブラリ](https://walkinglabs.github.io/learn-harness-engineering/ko/resources/) — テンプレート、チェックリスト、方法の参考資料
- [英語リソースライブラリ](https://walkinglabs.github.io/learn-harness-engineering/en/resources/) — templates, checklists, and method references
- [中国語リソースライブラリ](https://walkinglabs.github.io/learn-harness-engineering/zh/resources/) — 中文模板、清单和方法参考
- [ロシア語リソースライブラリ](https://walkinglabs.github.io/learn-harness-engineering/ru/resources/) — テンプレート、チェックリスト、参考資料
- [ベトナム語リソースライブラリ](https://walkinglabs.github.io/learn-harness-engineering/vi/resources/) — テンプレート、チェックリスト、参考資料

---

## エージェントセッションのライフサイクル

この講義の中核アイデアの1つは、**エージェントセッションは自由な流れではなく、構造化されたライフサイクルに従うべきだ**ということです。以下がその形です。

```text
    エージェントセッションのライフサイクル
    ======================

    ┌──────────────────────────────────────────────────────────────────┐
    │  START                                                          │
    │                                                                  │
    │  1. エージェントが AGENTS.md / CLAUDE.md を読む               │
    │  2. エージェントが init.sh を実行する (install, verify, health check) │
    │  3. エージェントが claude-progress.md を読む (前回何が起きたか) │
    │  4. エージェントが feature_list.json を読む (何が完了し、次は何か) │
    │  5. エージェントが git log を確認する (最近の変更)            │
    │                                                                  │
    │  SELECT                                                          │
    │                                                                  │
    │  6. エージェントが未完了の機能をちょうど1つだけ選ぶ          │
    │  7. エージェントはその機能だけに取り組む                     │
    │                                                                  │
    │  EXECUTE                                                         │
    │                                                                  │
    │  8. エージェントが機能を実装する                               │
    │  9. エージェントが検証を実行する (tests, lint, type-check)    │
    │  10. 検証に失敗したら: 修正して再実行                         │
    │  11. 検証に通ったら: 証拠を記録する                           │
    │                                                                  │
    │  WRAP UP                                                         │
    │                                                                  │
    │  12. エージェントが claude-progress.md を更新する             │
    │  13. エージェントが feature_list.json を更新する              │
    │  14. エージェントが、まだ壊れているものや未検証のものを記録する │
    │  15. エージェントが commit する (安全に再開できるときだけ)     │
    │  16. エージェントが次のセッション向けにクリーンな再開経路を残す │
    │                                                                  │
    └──────────────────────────────────────────────────────────────────┘

    ハーネスは、このライフサイクルのすべての遷移を統制します。
    モデルは、各段階でどのコードを書くかを決めます。
    ハーネスがなければ、9番目の手順は「エージェントが問題なさそうと言う」になります。
    ハーネスがあれば、9番目の手順は「テストが通る、lint がきれい、型チェックも通る」です。
```

---

## 対象読者

この講義は、次のような方を対象にしています。

- もっと高い安定性と品質を求めるコーディングエージェントの利用者
- ハーネス設計を体系的に理解したい研究者やビルダー
- 環境設計がエージェント性能にどう影響するかを理解する必要があるテックリード

この講義は、次のような方には向いていません。

- コードを書かない AI 入門を探している方
- プロンプトだけに興味があり、実装を構築するつもりがない方
- エージェントに実際のリポジトリで作業させる準備ができていない学習者

---

## 要件

この講義は、実際にコーディングエージェントを動かすための講義です。

次のツールのうち、少なくとも1つが必要です。

- Claude Code
- Codex
- ファイル編集、コマンド実行、複数段階の作業をサポートする、他の IDE または CLI コーディングエージェント

講義では、次のことができる前提です。

- ローカルリポジトリを開ける
- エージェントにファイルを編集させられる
- エージェントにコマンドを実行させられる
- 出力を確認して作業を再実行できる

そうしたツールがなくても講義内容は読めますが、意図どおりにプロジェクトを完了することはできません。

---

## ローカルプレビュー

このリポジトリはドキュメントビューアとして VitePress を使っています。

```sh
npm install
npm run docs:dev        # ホットリロード付きの開発サーバー
npm run docs:build      # 本番ビルド
npm run docs:preview    # ビルド済みサイトをプレビュー
```

VitePress が出力するローカル URL をブラウザで開いてください。

---

## 事前要件

必須:

- ターミナル、git、ローカル開発環境に慣れていること
- 一般的なアプリケーションスタックでコードを読み書きできること
- 基本的なソフトウェアデバッグ経験があること(ログ、テスト、実行時挙動を読めること)
- 実装中心の講義に十分な時間を割けること

あると助かるが必須ではないもの:

- Electron、デスクトップアプリ、またはローカルファーストツールの経験
- テスト、ロギング、またはソフトウェアアーキテクチャの背景知識
- Codex、Claude Code、または同種のコーディングエージェントに触れた経験

---

## 主要参考資料

主要:

- [OpenAI: Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/)
- [Anthropic: Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
- [Anthropic: Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps)

補足:

- [LangChain: The Anatomy of an Agent Harness](https://blog.langchain.com/the-anatomy-of-an-agent-harness/)
- [Thoughtworks: Harness Engineering](https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html)
- [HumanLayer: Skill Issue: Harness Engineering for Coding Agents](https://www.humanlayer.dev/blog/skill-issue-harness-engineering-for-coding-agents)

---

## リポジトリ構造

```text
learn-harness-engineering/
├── docs/                          # VitePress ドキュメントサイト
│   ├── lectures/                  # 12の講義 (index.md + code/ 例)
│   │   ├── lecture-01-*/
│   │   ├── lecture-02-*/
│   │   └── ... (全12件)
│   ├── projects/                  # 6つのプロジェクト説明
│   │   ├── project-01-*/
│   │   └── ... (全6件)
│   └── resources/                 # 多言語テンプレートと参考資料
│       ├── en/                    # 英語テンプレート、チェックリスト、ガイド
│       ├── zh/                    # 中国語テンプレート、チェックリスト、ガイド
│       ├── ru/                    # ロシア語テンプレート、チェックリスト、ガイド
│       └── vi/                    # ベトナム語テンプレート、チェックリスト、ガイド
├── projects/
│   ├── shared/                    # 共有 Electron + TypeScript + React ベース
│   └── project-NN/               # プロジェクトごとの starter/ と solution/ ディレクトリ
├── skills/                        # 再利用可能な AI エージェントスキル
│   └── harness-creator/           # ハーネスエンジニアリングスキル
├── package.json                   # VitePress + 開発ツール
└── CLAUDE.md                      # このリポジトリ向けの Claude Code 指示
```

---

## 講義の構成

- 各講義は1つの問いに集中します
- 講義には6つのプロジェクトが含まれます
- すべてのプロジェクトで、エージェントが実際の作業を行う必要があります
- すべてのプロジェクトで、弱いハーネスと強いハーネスの結果を比較します
- 重要なのは、書かれた文書の量ではなく、測定された差分です

---

## スター履歴

[![スター履歴チャート](https://api.star-history.com/svg?repos=walkinglabs/learn-harness-engineering&type=date&legend=top-left)](https://www.star-history.com/#walkinglabs/learn-harness-engineering&type=date&legend=top-left)

---

## 謝辞

この講義は [learn-claude-code](https://github.com/shareAI-lab/learn-claude-code) から着想を得て、アイデアを借りています。このプロジェクトは、単一ループから切り離された自律実行まで、エージェントをゼロから構築するための段階的なガイドです。
