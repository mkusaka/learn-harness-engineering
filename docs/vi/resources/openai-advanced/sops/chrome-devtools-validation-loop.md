# SOP: Chrome DevTools 検証ループ

UI の作業が実際の runtime 操作に依存していて、コードの確認だけでなく screenshot、DOM 状態、console 出力が重要な場合にこの SOP を使います。

## 目的

UI 検証を、journey がきれいに通るまで agent が繰り返し実行できる対話的なループに変えます。

## コアループ

1. 対象ページまたはアプリのバージョンを選ぶ。
2. 古い console ノイズを消す。
3. BEFORE の状態をキャプチャする。
4. UI のパスを起動する。
5. 操作中の runtime イベントを観察する。
6. AFTER の状態をキャプチャする。
7. 必要なら修正を適用してアプリを再起動する。
8. journey がきれいに通るまで検証をやり直す。

## 必須入力

- 安定して起動できるコマンド
- 再現可能な UI journey
- DOM、console、または screenshot を snapshot する手段
- 「きれい」と見なす基準

## 実行 SOP

1. 目標の journey を active の計画に書く。
2. 成功条件を、観察可能な言葉で定義する。たとえば、文言が表示される、ボタンが有効になる、エラーが消える、console がきれいになる、リクエストが成功する、など。
3. 操作前に初期状態を snapshot する。
4. 一度に起動する path は正確に 1 つだけにする。
5. runtime イベント、DOM 変化、見えている出力を記録する。
6. journey が失敗したら、責任範囲が最小の層を修正して再起動する。
7. 同じ path を再実行し、BEFORE / AFTER の証拠を比較する。

## きれいな状態の基準

- 想定した可視状態が表示されている
- 予期しないエラーがない
- console ノイズが理解済み、または削除済みである
- 同じ path を再実行しても同じ結果になる

## 更新対象の Artifact Repo

- active の実行計画
- journey が golden path になった場合は `docs/RELIABILITY.md`
- 可視の挙動が変わった場合は product spec
