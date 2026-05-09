# Project 01. プロンプトだけで agent にやらせるのと、ルールを決めてからやらせるのとで、どれくらい違うか

> 関連講義：[L01. モデルの能力が高いことは、実行の信頼性と同じではない](./../../lectures/lecture-01-why-capable-agents-still-fail/index.md) · [L02. Harness とは結局何か](./../../lectures/lecture-02-what-a-harness-actually-is/index.md)
> 本章のテンプレートファイル：[templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/zh/resources/templates/)

## 何をするか

Electron で最小限のナレッジベースアプリの外枠を作ります。ウィンドウを起動できて、左側に文書一覧、右側に Q&A パネルを表示し、ローカルにデータディレクトリを持つ構成です。タスク自体は難しくありません。難しいのは、これを agent にどう完了させるかです。

2 回実行します。1 回目はプロンプトだけを渡し、何も準備せずに agent がどこまでできるかを見ます。2 回目は事前にリポジトリへ `AGENTS.md`、`init.sh`、`feature_list.json` を置き、何をするか、どう検証するか、いつ完了とみなすかを構造化して agent に伝えます。そのうえで 2 回の結果を比較します。

このプロジェクトの核心はコードを書くことではなく、「最初に 15 分かけてルールを整えること」と「いきなり agent に任せること」の差をはっきりさせることです。

## 使うツール

- Claude Code または Codex（どちらか 1 つを選び、2 回とも同じものを使う）
- Git（ブランチ管理と比較）
- Node.js + Electron（项目技术栈）
- タイマー（各実行時間を記録する）

## 具体的な手順

### 準備

1. きれいな commit から始め、commit hash を記録します。
2. `p01-baseline` と `p01-improved` の 2 つのブランチを作成します。
3. 同じタスク用プロンプトを用意します。内容は次のとおりです。「Electron でナレッジベースアプリを作成してください。ウィンドウの左側は文書一覧エリア、右側は Q&A パネルエリアにし、アプリはローカルデータディレクトリを作成して使用する必要があります。」

### 1 回目の実行（弱い harness）

`p01-baseline` ブランチに切り替えます。

1. 上のプロンプトだけで agent を起動します。
2. `AGENTS.md` は渡さず、起動スクリプトも渡さず、受け入れ基準も用意しません。
3. 同じ時間上限とラウンド上限を設定します（推奨は 30 分 / 20 ラウンド）。
4. agent が止まったら、`npm start`（または該当する起動コマンド）を実行し、アプリが起動するか確認します。
5. 記録するもの: ターミナル出力、重要な diff、agent の最終サマリー。
6. **手でコードを修正しないでください**。起動しないなら起動しないまま、事実をそのまま記録します。

### 2 回目の実行（強い harness）

`p01-improved` ブランチに切り替えます。agent を起動する前に、リポジトリ内で次を準備します。

- `AGENTS.md`：プロジェクト構成、起動コマンド、Electron 層の境界ルールを明記する
- `init.sh`：ワンコマンドで実行可能な状態に戻す（`npm install && npm start`）
- `feature_list.json`：4 つの機能項目と、その完了状態を列挙する

そのあと、1 回目と同じプロンプト、同じ時間上限、同じラウンド上限で agent を起動します。agent が止まったら `./init.sh` を実行し、結果を記録します。

## 結果の測り方

| 指标 | 说明 |
|------|------|
| 完了状態 | 完了 / 一部完了 / 失敗 |
| 初回の起動成功までの時間 | 開始から `npm start` が初めて成功するまで |
| 再試行回数 | 起動できるまでに何回の手動介入が必要だったか |
| 取りこぼし | agent が完了を宣言した時点で、まだ実装されていなかった機能 |
| 早すぎる完了宣言 | 起動不能な状態で agent が完了を宣言していないか |

## 提出するもの

- 弱い harness の実行記録: プロンプト、ログ/対話記録、最終 diff、起動できた証拠
- 強い harness の実行記録: 同上に加え、準備した harness ファイル
- 比較メモ（1〜2 ページ）: 2 回の実行の差分、データ、結論

## 対応する講義

- [Lecture 01 — なぜ強い agent でも失敗するのか](../../lectures/lecture-01-why-capable-agents-still-fail/index.md)
- [Lecture 02 — harness とは結局何か](../../lectures/lecture-02-what-a-harness-actually-is/index.md)
- [Lecture 06 — なぜ初期化には独立したフェーズが必要なのか](../../lectures/lecture-06-why-initialization-needs-its-own-phase/index.md)
