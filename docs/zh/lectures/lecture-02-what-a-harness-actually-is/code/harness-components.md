# Harness Components Example

ローカルリポジトリで作業する coding agent の場合:

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

上記の harness を構成する要素のいずれかを変更すると、実際に動作する agent も変わります。
