[英語版](https://walkinglabs.github.io/learn-harness-engineering/en/) · [中国語](https://walkinglabs.github.io/learn-harness-engineering/zh/) · [ロシア語版](https://walkinglabs.github.io/learn-harness-engineering/ru/) · [ベトナム語版](https://walkinglabs.github.io/learn-harness-engineering/vi/) · [韓国語版](https://walkinglabs.github.io/learn-harness-engineering/ko/)

# Learn Harness Engineering

> **これは、環境、状態、検証、制御の仕組みを通じて、AI コーディングエージェント（Coding Agents）をより確実に動かす方法を体系的に学ぶプロジェクト型コースです。**

Learn Harness Engineering は、AI コーディングエージェントを実際の開発に落とし込むための方法に特化したコースです。本コースでは、業界最前線の Harness Engineering（ハーネスエンジニアリング）の理論と実践を深く研究・整理しており、参考資料には次のものが含まれます。

- [OpenAI: Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/)
- [Anthropic: Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
- [Anthropic: Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps)
- [Awesome Harness Engineering](https://github.com/walkinglabs/awesome-harness-engineering)

[**公式サイトとドキュメント (English & 中国語)**](https://walkinglabs.github.io/learn-harness-engineering/) | [**English README**](./README.md)

> **すぐ始めたいですか？** [`skills/harness-creator/`](./skills/) の skill を使えば、数分で自分のプロジェクトに本番品質の harness（AGENTS.md、機能一覧、init.sh、検証ワークフロー）を組み込めます。

---

<a id="界面速览"></a>
## ✨ 画面プレビュー

### 🏠 コースホーム
> コース全体のシラバスと核となる考え方をまとめ、明快な学習パスで素早く入門できます。

![コースホームのプレビュー](./docs/public/screenshots/readme/zh-home.png)

### 📖 没入感のある講義
> 実際の課題と実プロジェクト（Project 01 など）を組み合わせた深掘り解説で、没入感のある読み進めやすい学習体験を提供します。

![講義プレビュー](./docs/public/screenshots/readme/zh-lecture-01.png)

### 🗂️ そのまま使えるリソース集
> すぐに再利用できる中国語テンプレートと参考設定を整理し、AI Agent の多輪開発で起こりがちな「途中で止まる」「コンテキストが切れる」といった厄介な問題に効きます。

![拡張リソース集のプレビュー](./docs/public/screenshots/readme/zh-resources.png)

## PDF ビルドと Release パイプライン

このリポジトリには、コース PDF のビルド手順がすでに追加されています。

- ローカルで `npm run pdf:build` を実行すると、英語版と中国語版の両方のコース PDF が生成されます。
- 出力先は `artifacts/pdfs/` です。
- README 内のスクリーンショットを更新したい場合は、`npm run screenshots:readme` を実行してください。
- GitHub Actions のワークフロー [`release-course-pdfs.yml`](./.github/workflows/release-course-pdfs.yml) で PDF を自動ビルドし、成果物を GitHub Release にアップロードできます。

---

## モデルは強力だが、Harness がないと頼りにならない

多くの人が高い授業料を払ってようやく理解する事実があります。**世界最強のモデルでも、適切な作業環境がなければ、実際のエンジニアリングタスクではやはり失敗します。**

こういう経験はたぶんあるはずです。Claude や GPT にタスクを渡すと、一見うまくやっているように見えます。ファイルを読み、コードを書き、一生懸命に見える。ところが、問題が起こる。手順を一つ飛ばす。テストを壊す。「完了しました」と言ったのに、実際には何も動いていない。結局、後始末に費やす時間のほうが、自分でやるより長くなります。

これはモデルの問題ではありません。harness の問題です。

証拠ははっきりしています。Anthropic は対照実験を行いました。同じモデル（Opus 4.5）、同じプロンプト（「2D レトロゲームエディタを作る」）。harness なしでは、20 分で 9 ドルを使い、ゲームの核となる機能は動きませんでした。完全な harness（planner + generator + evaluator の 3-agent 構成）を加えると、6 時間で 200 ドルかかりましたが、実際に遊べるゲームができました。モデルは変わっていません。変わったのは harness です。

OpenAI も Codex を通じて同じ結論に至っています。harness がよく組まれたリポジトリでは、同じモデルが「不安定」から「信頼できる」へと変わります。単なる「少し良くなった」ではなく、質的な変化です。

**このコースでは、その環境をどう組むかを学びます。**

```text
                       HARNESS モード
                       ============

    あなた --> タスクを渡す --> agent が harness ファイルを読む --> agent が実行を開始
                                                        |
                                              harness が各ステップを制御：
                                              |
                                              +--> 指示：何を、どの順番でやるか
                                              +--> 範囲：一度に 1 つの機能、脱線しない
                                              +--> 状態：進捗ログ、機能一覧、git 履歴
                                              +--> 検証：テスト、lint、型チェック、スモークテスト
                                              +--> ライフサイクル：開始時に初期化、終了時に引き継ぎを残す
                                              |
                                              v
                                         agent は検証に合格してから
                                         だけ止まる
```

---

<a id="harness-engineering-到底是什么"></a>
## Harness Engineering とは何か

Harness engineering とは、モデルの周囲に一式の作業環境を組み立て、信頼できる結果を出させることです。より良いプロンプトを書くことだけが目的ではなく、モデルが動くシステム全体を設計することです。

ひとつの harness は、次の 5 つのサブシステムから成ります。

```text
    ┌─────────────────────────────────────────────────────────────────┐
    │                          HARNESS                                │
    │                                                                 │
    │   ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐ │
    │   │   指示       │  │     状態     │  │       検証           │ │
    │   │              │  │              │  │                      │ │
    │   │ AGENTS.md    │  │ progress.md  │  │ tests + lint         │ │
    │   │ CLAUDE.md    │  │ feature_list │  │ type-check           │ │
    │   │ feature_list │  │ git log      │  │ smoke runs           │ │
    │   │ docs/        │  │ session hand │  │ e2e pipeline         │ │
    │   └──────────────┘  └──────────────┘  └──────────────────────┘ │
    │                                                                 │
    │   ┌──────────────┐  ┌──────────────────────────────────────┐   │
    │   │     範囲      │  │          セッションのライフサイクル  │   │
    │   │              │  │                                      │   │
    │   │ 一度に 1 つ  │  │ 開始時に init.sh を実行             │   │
    │   │ の機能      │  │ 終了時にクリーンアップの確認を行う   │   │
    │   │ 明示的な完了 │  │ 次回のセッションへ引き継ぎメモを残す │   │
    │   │ 定義         │  │ 安全に再開できるときだけ commit する │   │
    │   └──────────────┘  └──────────────────────────────────────┘   │
    │                                                                 │
    └─────────────────────────────────────────────────────────────────┘

    モデルがどのコードを書くかを決める。
    Harness は、いつ、どこで、どう書くかを制御する。
    Harness はモデルを賢くはしない。
    ただし、出力を信頼できるものにする。
```

各サブシステムには役割があります。

- **指示** - agent に何を、どの順番で、作業前に何を読むかを伝えます。巨大な 1 ファイルではなく、段階的に展開される構造で、agent が必要に応じて辿れるようにします。
- **状態** - 何を終えたか、今何をしているか、次に何をするかを追跡します。ディスクに永続化され、次のセッションは前回止まったところから続けられます。
- **検証** - 合格したテストスイートだけが正解の証拠です。agent は、実行可能な証拠なしに「終わった」と言ってはいけません。
- **範囲** - agent が一度に 1 つの機能だけを扱うように制約します。やり過ぎも、やり残しも、未完了を隠すために機能一覧をこっそり書き換えることも許しません。
- **セッションのライフサイクル** - 開始時に初期化し、終了時に片付け、次のセッションがきれいに再開できる道筋を残します。

---

## なぜこのコースが必要なのか

問題は「モデルがコードを書けるか」ではありません。書けます。問題は、**本物のリポジトリで、複数回のセッションをまたいで、人がずっと付き添わなくても、実際の工程タスクを確実に完了できるか**です。

今の答えは、harness なしでは無理、です。

```text
    HARNESS なし                            HARNESS あり
    ==============                          ===========

    セッション 1: agent がコードを書く       セッション 1: agent が指示を読む
            agent がテストを壊す                     agent が init.sh を実行する
            agent が「完了しました」と言う           agent が 1 つの機能だけを実装する
            あなたが手作業で修復する                 agent が検証後にだけ完了を宣言する
                                                      agent が進捗ログを更新する
    セッション 2: agent が最初からやり直す          agent がきれいに commit する
            agent には前回の記憶がない
            agent が同じことをもう一度する          セッション 2: agent が進捗ログを読む
            あるいはまったく別のものを作る                 agent が前回止まったところから続ける
            あなたがまた直す                           agent が未完了の機能を続ける
                                                      あなたは火消しではなくレビューをする

    結果：あなたの時間が                      結果：agent が作業し、
          自分でやるよりかかる                     あなたが結果を検証する
```

このコースが本当に扱うテーマは次の通りです。

- どの harness 設計がタスク完了率を高めるのか？
- どの設計が手戻りと誤完了を減らすのか？
- どの仕組みが長時間タスクをより安定して前進させるのか？
- どの構造なら、複数回の agent 実行後もシステムを保守しやすいのか？

---

## コースの構成とドキュメント

コース全体の内容は、**[公式ドキュメントサイト (VitePress)](https://walkinglabs.github.io/learn-harness-engineering/zh/)** を直接ご覧ください。

このコースは 3 つの主要パートに分かれています。

1. **講義 (Lectures)**: 12 個の概念単元で、Harness 設計の理論的基盤を深く解説します。
2. **プロジェクト (Projects)**: 6 つの段階的な実践プロジェクトで、信頼できる Agent ワーク環境をゼロから組み立てていきます。
3. **リソースライブラリ (Resource Library)**: そのまま使える中英テンプレート（`AGENTS.md`、`feature_list.json`、`init.sh` など）を、今日から自分のプロジェクトにコピーして使えます。

---

<a id="快速开始今天就能改善你的-agent"></a>
## すぐ始める: 今日から agent を改善できる

12 本の講義を読み終えるまで待つ必要はありません。すでに coding agent で開発しているなら、以下の内容だけでもすぐに効果があります。

考え方はシンプルです。プロンプトだけで済ませるのではなく、リポジトリ内に構造化されたファイル群を置いて、agent に何をすべきか、何が終わったか、どう検証するかを伝えます。これらのファイルはプロジェクト内に置かれ、各セッションは同じ状態から始まります。

```text
    プロジェクトのルート
    ├── AGENTS.md              <-- agent の操作マニュアル
    ├── CLAUDE.md              <-- （Claude Code を使う場合の代替）
    ├── init.sh                <-- インストール + 検証 + 起動を 1 回で実行
    ├── feature_list.json      <-- どの機能があり、どれが完了したか
    ├── claude-progress.md     <-- 各セッションで何をしたか
    └── src/                   <-- あなたのコード
```

**Step 1.** まず [リソース集](https://walkinglabs.github.io/learn-harness-engineering/zh/resources/) に行き、上記ファイルのテンプレートを取得して、自分のプロジェクトのルートに置いてください。

これで十分です。最小構成は 4 ファイルあれば始められます。プロンプトだけに頼るより、Agent の挙動はずっと安定します。

---

<a id="贯穿项目一个真实的应用"></a>
## プロジェクトを通して続く: ひとつの実例

6 つのコースプロジェクトはすべて、同じ製品を中心にしています。**Electron ベースの個人知識ベース・デスクトップアプリ**です。

```text
    ┌─────────────────────────────────────────────────────┐
    │                知識ベース・デスクトップアプリ        │
    │                                                     │
    │  ┌──────────────┐  ┌──────────────────────────────┐│
    │  │  ドキュメント一覧 │  │        Q&A パネル        ││
    │  │              │  │                              ││
    │  │ doc-001.md   │  │  質問: harness eng とは？    ││
    │  │ doc-002.md   │  │  回答: agent を囲む        ││
    │  │ doc-003.md   │  │      作業環境...           ││
    │  │ ...          │  │      [引用: doc-002.md]    ││
    │  └──────────────┘  └──────────────────────────────┘│
    │                                                     │
    │  ┌─────────────────────────────────────────────────┐│
    │  │ ステータスバー: 42 件の文書 | 38 件を索引済み | 最終同期 3 分前 ││
    │  └─────────────────────────────────────────────────┘│
    └─────────────────────────────────────────────────────┘

    コア機能：
    ├── ローカル文書を取り込む
    ├── 文書ライブラリを管理する
    ├── 文書を処理して索引付けする
    ├── 取り込んだ内容に対して AI Q&A を実行する
    └── 引用付きで追跡可能な回答を返す
```

このプロジェクトを選んだのは、実用性の高さ、十分な製品複雑性、そして harness の改善前後の差を観察しやすさを、同時に満たしているからです。

各コースプロジェクトの starter/solution は、この Electron アプリをそれぞれの進化段階で丸ごと複製したものです。P(N+1) の starter は P(N) の solution から派生します。アプリが、あなたの harness スキルと一緒に進化していきます。

---

<a id="学习路径"></a>
## 学習パス

このコースは順番どおりに進めるのが最も効果的です。各段階は前の段階の上に積み上がります。

```text
    段階 1: 問題を認識する                      段階 2: リポジトリを整理する
    ===================                     ===================

    L01  モデルが強い ≠ 実行が信頼できる       L03  リポジトリを唯一の
                                                     事実源にする
    L02  Harness とは何か
                                           L04  指示を複数の
         |                                      ファイルに分割する
         v
    P01  プロンプトだけを書く                    |
         vs ルールを決める                        v
                                                   P02  agent が読める作業空間


    段階 3: セッションをつなぐ                段階 4: フィードバックと範囲
    ===================                     ===================

    L05  跨いだタスクでも                         L07  タスク境界を明確にする
         継続性を保つ
                                           L08  機能一覧を
    L06  毎回の開始前に                             harness の原語にする
         まず初期化する
                                                 |
         |                                       v
         v                                       P04  実行フィードバックで
    P03  セッション間の継続性                          agent の振る舞いを修正する


    段階 5: 検証                              段階 6: すべてをつなぐ
    ===================                     ===================

    L09  agent の早すぎる                          L11  agent の実行過程を
         勝利宣言を防ぐ                               観測可能にする

    L10  完全なフローを                            L12  各セッションの終了前に
         通してこそ本当の検証になる                   引き継ぎを整える

         |                                       |
         v                                       v
    P05  agent が自分の仕事を検証する             P06  完全な harness を構築する
                                                   （統合プロジェクト）
```

各段階はおおむね 1 週間程度です（副業ペース）。急ぐなら、最初の 3 段階は週末 1 回で終えられます。

---

<a id="课程大纲"></a>
## コース概要

### 講義 - 12 の概念単元で、それぞれが 1 つの核心的な問いに答える

*すべての講義は [ドキュメントサイト](https://walkinglabs.github.io/learn-harness-engineering/zh/) から読めます。*

| 講義 | 中心となる問い | 重要概念 |
| ---- | -------- | -------- |
| [L01](./docs/zh/lectures/lecture-01-why-capable-agents-still-fail/index.md) | モデルの能力が高いことは、実行の信頼性を意味しない | ベンチマークと実際のエンジニアリングの間にある能力ギャップ |
| [L02](./docs/zh/lectures/lecture-02-what-a-harness-actually-is/index.md) | Harness とは何か | 5 つのサブシステム: 指示、状態、検証、範囲、ライフサイクル |
| [L03](./docs/zh/lectures/lecture-03-why-the-repository-must-become-the-system-of-record/index.md) | リポジトリを唯一の事実源にする | agent が見えないものは、agent にとって存在しない |
| [L04](./docs/zh/lectures/lecture-04-why-one-giant-instruction-file-fails/index.md) | なぜ巨大な指示ファイルは失敗するのか | 段階的展開: 地図は渡すが、百科事典は渡さない |
| [L05](./docs/zh/lectures/lecture-05-why-long-running-tasks-lose-continuity/index.md) | なぜ長時間タスクは文脈を失うのか | 進捗をディスクに永続化し、止まったところから再開する |
| [L06](./docs/zh/lectures/lecture-06-why-initialization-needs-its-own-phase/index.md) | なぜ初期化には独立した段階が必要なのか | agent が作業を始める前に、環境が健全かを検証する |
| [L07](./docs/zh/lectures/lecture-07-why-agents-overreach-and-under-finish/index.md) | なぜ agent はやり過ぎたり、やり切れなかったりするのか | 1 回に 1 つの機能、明示的な完了定義 |
| [L08](./docs/zh/lectures/lecture-08-why-feature-lists-are-harness-primitives/index.md) | なぜ機能一覧は harness の原語なのか | machine-readable な範囲境界は agent が無視できない |
| [L09](./docs/zh/lectures/lecture-09-why-agents-declare-victory-too-early/index.md) | なぜ agent は早く完了宣言してしまうのか | 検証の抜け: 自信 ≠ 正しさ |
| [L10](./docs/zh/lectures/lecture-10-why-end-to-end-testing-changes-results/index.md) | なぜ E2E テストは結果を変えるのか | 完全なフローを通してこそ、はじめて検証になる |
| [L11](./docs/zh/lectures/lecture-11-why-observability-belongs-inside-the-harness/index.md) | なぜ可観測性は harness の内部に属するのか | agent が何をしたか見えなければ、壊したものを直せない |
| [L12](./docs/zh/lectures/lecture-12-why-every-session-must-leave-a-clean-state/index.md) | なぜ毎回のセッションはきれいな状態を残す必要があるのか | 次のセッションの成功は、今回のクリーンアップ次第 |

### プロジェクト - 6 つの実践プロジェクトで、講義の方法を同じ Electron アプリに落とし込む

| プロジェクト | やること | Harness の仕組み |
| ---- | ---------- | ------------ |
| [P01](./docs/zh/projects/project-01-baseline-vs-minimal-harness/index.md) | 同じタスクを 2 回実行する: プロンプトだけ vs ルールを決める | 最小構成の harness: AGENTS.md + init.sh + feature_list.json |
| [P02](./docs/zh/projects/project-02-agent-readable-workspace/index.md) | agent が理解できるようにプロジェクト構造を組み替える | agent が読める作業空間 + 永続化された状態ファイル |
| [P03](./docs/zh/projects/project-03-multi-session-continuity/index.md) | agent を閉じても再起動して続けられるようにする | 進捗ログ + セッション引き継ぎ + 複数セッションの継続性 |
| [P04](./docs/zh/projects/project-04-incremental-indexing/index.md) | agent のやり過ぎ・やり足りなさを防ぐ | 実行フィードバック + 範囲制御 + 増分インデックス |
| [P05](./docs/zh/projects/project-05-grounded-qa-verification/index.md) | agent に自分の作業を自分で検証させる | 自己検証 + 引用付き Q&A + 証拠に基づく完了判定 |
| [P06](./docs/zh/projects/project-06-runtime-observability-and-debugging/index.md) | ゼロから完全な harness を組み上げる（統合プロジェクト） | 完全な harness: 全機構 + 可観測性 + アブレーション実験 |

```text
    プロジェクトの進化
    ========

    P01  プロンプトだけ vs ルールを決める       問題を認識する
     |
     v
    P02  agent が読める作業空間               リポジトリを整理する
     |
     v
    P03  複数セッションの継続性               セッションをつなぐ
     |
     v
    P04  実行フィードバックと範囲制御          フィードバックループを追加する
     |
     v
    P05  自己検証                             agent に自分をチェックさせる
     |
     v
    P06  完全な harness（統合プロジェクト）     完全なシステムを組み上げる

    各プロジェクトの solution が、次のプロジェクトの starter になる。
    アプリはあなたの harness スキルとともに進化する。
```

### リソースライブラリ

- [中国語版リソース集](https://walkinglabs.github.io/learn-harness-engineering/zh/resources/) - 中国語テンプレート、チェックリスト、方法の参考資料
- [英語版リソース集](https://walkinglabs.github.io/learn-harness-engineering/en/resources/) - 英語テンプレート、チェックリスト、方法の参考資料

---

<a id="其他课程"></a>
## その他のコース

私たちのチームは、ほかのコースも制作しています。ぜひご覧ください。

[![Hands-on Modern RL](https://img.shields.io/badge/HANDS--ON_MODERN_RL-0052cc?style=for-the-badge)](https://github.com/walkinglabs/hands-on-modern-rl)

**Hands-on Modern RL**: 基礎的な RL の概念から、LLM alignment、RLVR、さらに高度な Agentic systems へとつなぐ、オープンソースの実践型カリキュラムです。

---

## スター履歴

[![スター履歴チャート](https://api.star-history.com/svg?repos=walkinglabs/learn-harness-engineering&type=date&legend=top-left)](https://www.star-history.com/#walkinglabs/learn-harness-engineering&type=date&legend=top-left)

---

## 謝辞

このコースは、[learn-claude-code](https://github.com/shareAI-lab/learn-claude-code) にある発想や一部の考え方から着想を得ています。これは、単一ループから分離された自律実行までを段階的に組み立てる agent 入門チュートリアルです。

---

<a id="skills"></a>
## スキル

このリポジトリには再利用可能な AI Agent スキルも含まれており、IDE や Agent ワークスペースにそのままインストールできます。

- [**harness-creator**](./skills/harness-creator/)：自分のプロジェクトに本番品質の harness を数分で組み立てるのを助けるスキルです。

---

## Agent のセッションライフサイクル

このコースの核となる考え方のひとつは、**agent のセッションは自由に振る舞うのではなく、構造化されたライフサイクルに従うべきだ**ということです。流れは次の通りです。

```text
    AGENT セッションライフサイクル
    =================

    ┌──────────────────────────────────────────────────────────────────┐
    │  開始                                                            │
    │                                                                  │
    │  1. agent が AGENTS.md / CLAUDE.md を読む                        │
    │  2. agent が init.sh を実行する（インストール、検証、健全性確認）│
    │  3. agent が claude-progress.md を読む（前回何をしたか）         │
    │  4. agent が feature_list.json を読む（完了済みと次の対象を確認）│
    │  5. agent が git log を確認する（最近の変更）                    │
    │                                                                  │
    │  選択                                                            │
    │                                                                  │
    │  6. agent が未完了の機能を 1 つだけ選ぶ                          │
    │  7. agent はその 1 つだけを実装する                              │
    │                                                                  │
    │  実行                                                            │
    │                                                                  │
    │  8. agent がその機能を実装する                                   │
    │  9. agent が検証（テスト、lint、型チェック）を走らせる          │
    │  10. 検証に失敗したら: 修正して再実行                           │
    │  11. 検証に通ったら: 証拠を記録する                              │
    │                                                                  │
    │  仕上げ                                                          │
    │                                                                  │
    │  12. agent が claude-progress.md を更新する                      │
    │  13. agent が feature_list.json を更新する                       │
    │  14. agent が未完了と未検証の項目を記録する                      │
    │  15. agent が commit する（安全に再開できる場合のみ）            │
    │  16. agent が次回セッション向けにきれいな再開経路を残す         │
    │                                                                  │
    └──────────────────────────────────────────────────────────────────┘

    Harness はこのライフサイクルのあらゆる状態遷移を制御する。
    何を書くかを決めるのはモデル。
    harness がないと、9 歩目は「たぶん大丈夫に見える」で終わる。
    harness があれば、9 歩目は「テスト合格、lint クリア、型チェック合格」になる。
```

---

## 対象読者

このコースは次のような人に向いています。

- すでに coding agent を使っていて、安定性と品質を上げたいエンジニア
- harness 設計を体系的に理解したい研究者やビルダー
- 「環境設計が agent の性能にどう影響するか」を理解する必要がある技術責任者

このコースは次のような人には向きません。

- コードを書かない AI 入門だけを求めている人
- prompt だけに関心があり、実装するつもりがない人
- agent を実際のリポジトリで動かすつもりがない学習者

---

## 環境要件

これは、本当に coding agent を動かしながら学ぶコースです。

少なくとも次のいずれかが必要です。

- Claude Code
- Codex
- ファイル編集、コマンド実行、複数ステップのタスクに対応したその他の IDE / CLI coding agent

コースでは、次のことができる前提です。

- ローカルリポジトリを開ける
- agent にファイル編集を許可できる
- agent にコマンド実行を許可できる
- 出力を確認しながら、タスクを繰り返し実行できる

このようなツールがない場合でも内容は読めますが、想定どおりにコースプロジェクトを完了することはできません。

---

## ローカルプレビュー

このリポジトリは VitePress をドキュメントビューアとして使っています。

```sh
npm install
npm run docs:dev        # 開発サーバー（ホットリロード）
npm run docs:build      # 本番ビルド
npm run docs:preview    # ビルド結果のプレビュー
```

その後、ブラウザで VitePress が出力するローカルアドレスを開いてください。

---

## 前提知識

必須:

- ターミナル、git、ローカル開発環境に慣れていること
- 一般的なアプリケーションスタックのコードを、少なくとも 1 つ読んで書けること
- ログ、テスト、実行挙動の見方を含む、基本的なソフトウェアデバッグ経験があること
- 実装寄りのコース課題をこなせるだけの時間を確保できること

あると役立つが必須ではないもの:

- Electron、デスクトップアプリ、ローカルファーストツールを使った経験
- テスト、ログ、ソフトウェアアーキテクチャの経験
- Codex、Claude Code、あるいは類似の coding agent をすでに触ったことがある

---

## 主要参考資料

主な参考資料:

- [OpenAI: Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/)
- [Anthropic: Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
- [Anthropic: Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps)
- [OpenAI: Unrolling the Codex agent loop](https://openai.com/index/unrolling-the-codex-agent-loop/)
- [Anthropic: Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)
- [LangChain: Improving Deep Agents with harness engineering](https://www.langchain.com/blog/improving-deep-agents-with-harness-engineering)
- [Thoughtworks / Martin Fowler: Harness engineering for coding agent users](https://martinfowler.com/articles/harness-engineering.html)
- [Cursor: Continually improving our agent harness](https://cursor.com/blog/continually-improving-agent-harness)

完全な階層別の参考一覧は [`docs/zh/resources/reference/`](./docs/zh/resources/reference/index.md) をご覧ください。

---

## リポジトリ構成

```text
learn-harness-engineering/
├── docs/                          # VitePress ドキュメントサイト
│   ├── index.md                   # 言語選択ページ（中国語 / English）
│   ├── zh/                        # 中国語版の完全なコンテンツ
│   │   ├── lectures/              # 12 講義（index.md + code/ 例）
│   │   ├── projects/              # 6 プロジェクト
│   │   └── resources/             # テンプレート、参考資料、高度な資料パック
│   └── en/                        # 英語版の完全なコンテンツ
│       ├── lectures/              # 12 講義
│       ├── projects/              # 6 プロジェクト
│       └── resources/             # テンプレート、参考資料、高度な資料パック
├── projects/
│   ├── shared/                    # Electron + TypeScript + React の共通ベース
│   └── project-NN/               # 各プロジェクトの starter/ と solution/
├── skills/                        # 再利用可能な AI agent スキル
│   └── harness-creator/           # Harness エンジニアリング用スキル
├── package.json                   # VitePress + 開発ツール
└── CLAUDE.md                      # Claude Code の指示ファイル
```

---
