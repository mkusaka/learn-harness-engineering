# ハーネスの構成要素の例

ローカルのリポジトリで動作する coding agent の場合:

- モデル:
  LLM そのもの

- ハーネス:
  - system prompt
  - AGENTS.md
  - bash tool
  - ファイル読み書きツール
  - git アクセス権
  - ローカルファイルシステム
  - 起動スクリプト
  - テストコマンド
  - stop hook
  - lint チェック
  - evaluator のループ

上のハーネスのどの部分を変えても、実際の agent が変わります。
