# SOP: Observability フィードバックループ

デバッグが遅いとき、agent が証拠なしに成功を主張し続けるとき、あるいは runtime の挙動がコード自体よりも検証しづらいときに、この SOP を使います。

## 目的

log、metrics、trace、そして実行可能な workload を通じて、agent にローカルなフィードバックループを与え、コードの確認だけでなく実行結果からも推論できるようにします。

## 最小限のスタック

- 構造化 log を出力するアプリケーション
- 可能であれば metrics と trace を出力するアプリケーション
- ローカルの fan-out 層または収集層
- log、metrics、trace を問い合わせるための interface
- 各変更後に再実行できる、再現可能な workload または user journey

## 実行 SOP

1. 重要な golden runtime journey を定義します。
2. 起動処理と重要な path に構造化 log を追加します。
3. 有用な場合は、latency、failure count、queue depth の metrics を追加します。
4. 遅い処理や複数段階の flow には trace または timing marker を追加します。
5. これらの signal をローカルの dev 環境から問い合わせ可能にします。
6. 各変更後に再実行できる workload または scenario を agent に用意します。
7. ループを要求します: query -> correlate -> reason -> implement -> restart -> rerun -> verify。

## Debug セッションのチェックリスト

- 何が失敗しましたか?
- 失敗を裏付ける signal はどれですか?
- どの layer がその失敗の責任を持っていますか?
- 修正後に何が変わりましたか?
- application は clean に再起動できましたか?
- 同じ workload は rerun 後に成功しましたか?

## 完了条件

- agent が runtime の evidence に基づいて failure mode を説明できる。
- 同じ workload を各変更後に再実行できる。
- restart と rerun が通常の task loop の一部になっている。
- reliability signal が `docs/RELIABILITY.md` に記録されている。
