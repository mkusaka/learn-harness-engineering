# SOP: リポジトリに見えない知識を埋め込む

重要な文脈がまだ Google Docs、チャットスレッド、チケット、あるいは人の頭の中に残っているときに、このSOPを使います。

## 目的

エージェントから見えない知識をコードベース内で見つけられる形にし、新しいセッションでも過去の会話に頼らずに対応できるようにします。

## 発動条件

- エージェントがシステムの動作について何度も尋ねてくる。
- 人間が「これは Slack で決めた」「先週 X が言ったことに従って」と言う。
- レビューで、リポジトリ内に書かれていないプロダクト規則やセキュリティ規則が参照される。
- 新しいセッションが、本来ならすでに確定しているはずの調査作業を繰り返す。

## 実行手順

1. 見えない知識の出どころを列挙します。docs、チャット、暗黙のチームルール、口頭での決定などです。
2. 各出どころについて、それが architecture、product behavior、security policy、reliability expectation、plan context、reference material のどれに当たるかを確認します。
3. 該当する repo artifact に書き込みます。
   - architecture -> `ARCHITECTURE.md`
   - product behavior -> `docs/product-specs/`
   - design rationale -> `docs/design-docs/`
   - execution state -> `docs/exec-plans/`
   - repeated external references -> `docs/references/`
   - quality or reliability expectations -> `docs/QUALITY_SCORE.md` or `docs/RELIABILITY.md`
4. 曖昧な表現を、運用上そのまま使える表現に置き換えます。
5. 古い複製は削除するか非推奨にして、リポジトリに参照可能な真実を1つだけ残します。

## 良い記述ルール

- 発見しやすさを優先し、文学的な完成度は追い求めない。
- 短く、ファイル名が明確な文書を優先する。
- 関連する artifact 同士をリンクでつなぐ。
- 会議の逐語録ではなく、長く有効なルールを保存する。
- 決定が行われたそのセッションで、リポジトリも更新する。

## 完了条件

- 新しいエージェントが、人間に聞かずに関連ルールを見つけられる。
- 同じ事実が、矛盾する複数のファイルに散らばっていない。
- 新しい artifact が、それが管理するコードやワークフローの近くに置かれている。
