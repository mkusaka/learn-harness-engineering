# Harness Components Example

ローカルリポジトリで動作する coding agent にとって:

- Model:
  LLM そのもの

- Harness:
  - system prompt
  - AGENTS.md
  - bash tool
  - file read/write tools
  - git access
  - local filesystem
  - startup scripts
  - test commands
  - stop hooks
  - lint checks
  - evaluator loop

上記の harness の要素のいずれかを変更すると、実際の agent も変わります。
