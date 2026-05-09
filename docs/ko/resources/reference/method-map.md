# メソッドマップ (Method Map)

この表は、長期稼働するコーディングエージェント(agent)の作業でよく起きる失敗タイプ(failure mode)を、通常はそれを最初に解決する成果物(artifact)または運用ルール(operating rule)に対応付けます。新しい問題を見つけたときに、全体向けのガイドラインファイルへさらに文章を足すのではなく、その失敗タイプに直接対応する最小限の成果物を追加するという原則に従います。

| 失敗タイプ (Failure mode) | 実際の様子 (What it looks like in practice) | 主な修正方法 (Primary fix) | 補助成果物 (Supporting artifact) |
| --- | --- | --- | --- |
| コールドスタートの混乱 (Cold-start confusion) | 新しいセッションが、設定と状態の再発見に大半の時間を使ってしまう | リポジトリを system of record にする | `claude-progress.md` |
| スコープの拡散 (Scope sprawl) | エージェントが複数の機能に着手するが、どれもきれいに完了できない | 有効なスコープを制限する | `feature_list.json` |
| 早すぎる完了宣言 (Premature completion) | エージェントがコード編集のあと、実行可能な証拠(runnable proof)なしに完了を主張する | 完了を証拠に結び付ける | `clean-state-checklist.md` |
| 脆い起動 (Fragile startup) | どのセッションも、プロジェクトの起動方法を毎回学び直す | 設定と検証を標準化する | `init.sh` |
| 弱いハンドオフ(Weak handoff) | 次のセッションが、何が検証済みで、何が壊れていて、次に何をすべきか分からない | 明示的なハンドオフ(handoff)で終了する | `session-handoff.md` |
| 主観的なレビュー (Subjective review) | レビュー品質が好みや記憶に依存してしまう | 固定カテゴリで成果物を採点する | `evaluator-rubric.md` |

## 運用原則 (Operating Principle)

観測された失敗タイプ(failure mode)を直接解決する最小限の成果物を追加してください。1つの全体向けガイドラインファイルに、さらに大量の文章をダンプ(dumping)して、あらゆる信頼性の問題を解決しようとしないでください。
