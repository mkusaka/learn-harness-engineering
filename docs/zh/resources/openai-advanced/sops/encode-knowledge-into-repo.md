# SOP: リポジトリに見えない知識をコード化する

重要な文脈がまだ Google Docs、チャットのやり取り、ticket、あるいは人の頭の中に散らばっているなら、この SOP を使ってください。

## 目的

これまで agent から見えなかった知識を、codebase の中で見つけられ、読めて、実行できる事実に変えること。

## 発火条件

- agent がシステムの動きを何度も聞き返してくる。
- 人が「それは Slack で決まっていた」「先週のあの人の指示に従って」と言い始める。
- review で、リポジトリには書かれていない product、security、architecture のルールが頻繁に参照される。
- 新しい会話のたびに、本来は定着しているはずの発見作業を繰り返している。

## SOP の実行

1. まず、見えない知識の出どころをすべて洗い出す。外部ドキュメント、チャット、チームのデフォルトルール、口頭での決定など。
2. 各知識について、それが architecture、product の振る舞い、security policy、reliability 要件、実行時の文脈、参照資料のどれに当たるかを判断する。
3. 分類に応じて、対応する repository artifact に書き込む。
   - architecture -> `ARCHITECTURE.md`
   - product の振る舞い -> `docs/product-specs/`
   - 設計上の理由 -> `docs/design-docs/`
   - 実行状態 -> `docs/exec-plans/`
   - 外部参照資料 -> `docs/references/`
   - quality または reliability の要件 -> `docs/QUALITY_SCORE.md` または `docs/RELIABILITY.md`
4. 曖昧な表現は、実際の運用で本当に役立つ表現に言い換える。
5. 古いコピーは削除または廃止し、リポジトリ内に discoverable な唯一の truth source を保つ。

## 良い記述ルール

- 「見つけやすさ」のために書くのであって、「細かく全部書く」ために書くのではない。
- ファイルはできるだけ短く、名前はできるだけ明確にする。
- 関連する artifact 同士は相互にリンクする。
- 耐久性のあるルールを記録し、会議の逐語録をそのまま貼り付けない。
- 決定が固まったその会話のうちに、repository を更新する。

## 完了条件

- 新しく参加した agent が、人に聞かずに関連ルールを見つけられる。
- 同じ事実が、互いに矛盾する複数のファイルに散らばらない。
- 新しい artifact は、それが管理する code や process に最も近い場所に置かれている。
