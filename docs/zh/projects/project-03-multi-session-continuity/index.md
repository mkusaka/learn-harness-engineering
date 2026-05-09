# Project 03. agent を一度止めて再開しても作業を続けられるようにする

> 関連講義：[L05. なぜ長時間タスクはコンテキストを失うのか](./../../lectures/lecture-05-why-long-running-tasks-lose-continuity/index.md) · [L06. なぜ初期化には独立したフェーズが必要なのか](./../../lectures/lecture-06-why-initialization-needs-its-own-phase/index.md)
> 本篇テンプレートファイル：[templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/zh/resources/templates/)

## 何をするか

P02 で「引き継ぎ」の問題は解決しましたが、agent が引き継いだあとに最後まで正しくやり切れるかは、また別の問題です。このプロジェクトでは、agent にスコープ制御と検証の関門を追加します。

実装するナレッジベース機能は、文書の分割、メタデータ抽出、インデックス進捗の表示、引用付きの Q&A フローです。これらは前の 2 つのプロジェクトより複雑なので、agent はより簡単に逸脱します。やるべきでないことまで実行してしまったり、完了したと言いながら実際には検証を通っていなかったりします。

`feature_list.json` を用意し、各機能の状態を明確に `failing` / `passing` で管理します。ルールは単純です。1 回に 1 機能だけ進め、実行可能な検証の証拠がなければ `passing` にしてはいけません。2 回実行し、1 回はこれらの制約を与えず、もう 1 回は厳格に適用して、結果の差を確認します。

## 使うツール

- Claude Code または Codex
- Git
- Node.js + Electron
- `feature_list.json`（テンプレートは `docs/zh/resources/templates/feature_list.json` を参照）

## 具体的な手順

### 準備

1. P02 完了後のコードを基点にする。
2. `p03-baseline` と `p03-improved` の 2 つのブランチを作成する。
3. 4 つの機能を定義する。文書分割、メタデータ抽出、インデックス進捗 UI の表示、引用付き Q&A。両ブランチで機能定義は完全に同一にする。

### 1 回目の実行（弱い harness）

`p03-baseline` ブランチに切り替える。

1. agent を起動し、あいまいなタスク指示を与える。
2. `feature_list.json` は渡さず、状態追跡も行わない。
3. agent が一度にいくつの機能を進めても制限しない。
4. 明確な検証基準も設けない。agent が「完了した」と言えば完了扱いにする。
5. 実行後、各機能が本当に使えるかを手動で確認する。
6. agent が完了したと主張したのに、実際には検証を通っていなかった機能を記録する。

### 2 回目の実行（強い harness）

`p03-improved` ブランチに切り替える。agent を起動する前に次を行う。

- リポジトリルートに `feature_list.json` を置き、4 つの機能をすべて `failing` にする。
- `AGENTS.md` にルールを書く。1 回に 1 機能だけ進めること。状態は `failing` から `passing` にしか移せず、移すには検証証拠が必要であること。
- `init.sh` を準備する。

そのうえで agent を起動する。

1. agent が作業を進め、機能を 1 つ終えるたびに `feature_list.json` を更新し、検証証拠（スクリーンショット、テスト出力など）を添付する。
2. 少なくとも 1 つの機能で、`failing` から `passing` への完全な移行過程を示す。
3. Q&A 機能の検証では、引用が存在することと、その引用が関連していることの両方を確認し、出力があるかどうかだけでは判断しない。
4. 実行後、すべての検証証拠をアーカイブする。

## 評価方法

| 指標 | 説明 |
|------|------|
| スコープ逸脱回数 | agent が機能一覧に含まれない作業を行った回数 |
| 虚偽完了率 | 完了したと主張した機能のうち、検証に通らなかった割合 |
| 検証カバレッジ | 明確な検証証拠がある機能の、全機能に対する割合 |
| Q&A 品質 | 引用が存在するか、引用が関連しているか |
| リトライ回数 | 開始からすべての機能が `passing` になるまでに再試行した回数 |

## 提出物

- 弱い harness の実行記録: プロンプト、ログ、検証結果
- 強い harness の実行記録: `feature_list.json` の変更履歴、ログ、検証証拠
- 少なくとも 1 つの機能が `failing` から `passing` に移行した証拠
- 比較メモ: スコープ規律と完了の正確さを重点的に見る

## 対応講義

- [Lecture 07 — なぜ agent はやりすぎたり、やり切れなかったりするのか](../../lectures/lecture-07-why-agents-overreach-and-under-finish/index.md)
- [Lecture 08 — なぜ feature list が harness の基礎プリミティブなのか](../../lectures/lecture-08-why-feature-lists-are-harness-primitives/index.md)
- [Lecture 09 — なぜ agent は早すぎる勝利宣言をしてしまうのか](../../lectures/lecture-09-why-agents-declare-victory-too-early/index.md)
- [Lecture 10 — なぜ end-to-end テストが結果を変えるのか](../../lectures/lecture-10-why-end-to-end-testing-changes-results/index.md)
