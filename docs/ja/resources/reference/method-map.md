# Method Map

この表は、長時間動作する coding agent によくある失敗パターンを、
最初に効くことが多い成果物または運用ルールに対応付けたものです。

| Failure mode | 実際にはどう見えるか | Primary fix | Supporting artifact |
| --- | --- | --- | --- |
| Cold-start confusion | 新しいセッションが、セットアップや状況の再確認にほとんどの時間を使ってしまう | リポジトリを system of record にする | `claude-progress.md` |
| Scope sprawl | エージェントが複数の機能に手を付けるが、どれもきれいに完了しない | アクティブな範囲を絞る | `feature_list.json` |
| Premature completion | コードを編集しただけで、実行可能な証拠が出る前に完了を宣言してしまう | 完了条件を証拠に結び付ける | `clean-state-checklist.md` |
| Fragile startup | 毎回のセッションで、プロジェクトの起動方法を最初から学び直す | セットアップと検証を標準化する | `init.sh` |
| Weak handoff | 次のセッションが、何が検証済みで、何が壊れていて、次に何をするべきか分からない | 明示的な引き継ぎで終える | `session-handoff.md` |
| Subjective review | レビュー品質が、好みや記憶に左右される | 固定カテゴリで出力を採点する | `evaluator-rubric.md` |

## Operating Principle

観測された失敗パターンに直接効く、最小限の成果物を追加してください。
信頼性の問題をすべて、1つのグローバルな指示ファイルに文章を足すことで
解決しようとしないでください。
