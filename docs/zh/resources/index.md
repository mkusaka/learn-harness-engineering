# 中文资料库

このフォルダは、コースで扱う手法をそのまま参照・再利用できる形に整理したものです。概念を繰り返し説明するのではなく、できるだけ次の2点に答えることを目的にしています。

- 今すぐ最初にどのファイルを真似ればよいか
- それぞれのファイルが何を解決するのか

## いつ使うか

Codex、Claude Code、または他の coding agent を1つのリポジトリで継続的に動かしたいときは、ここから始めるとよいです。特に次のような場面に向いています。

- 複数回の会話をまたぐ開発で、コンテキストが途切れるのが心配なとき
- 機能数が多く、中途半端な状態のまま止まりやすいとき
- できたと早めに宣言しがちだが、実際には十分にテストできていないとき
- 毎回作業を始めるたびに、起動方法を一から思い出す必要があるとき

## ここから始める

まず最小構成で始めたいなら、次のファイルを優先して確認してください。

- ルート指示ファイル：[`templates/AGENTS.md`](./templates/AGENTS.md) または [`templates/CLAUDE.md`](./templates/CLAUDE.md)
- 機能状態ファイル：[`templates/feature_list.json`](./templates/feature_list.json)
- 進捗ログ：[`templates/claude-progress.md`](./templates/claude-progress.md)
- 起動スクリプトの参考：`docs/resources/templates/init.sh`

そのうえで、必要に応じて次を追加します。

- 会話引き継ぎテンプレート：[`templates/session-handoff.md`](./templates/session-handoff.md)
- 終了時チェックリスト：[`templates/clean-state-checklist.md`](./templates/clean-state-checklist.md)
- レビューテンプレート：[`templates/evaluator-rubric.md`](./templates/evaluator-rubric.md)

OpenAI のあの harness engineering 記事で紹介されている、より完全なリポジトリ構成をそのまま採用したいなら、こちらも参照してください。

- [`openai-advanced/index.md`](./openai-advanced/index.md)

## 資料庫の構成

- [`templates/`](./templates/index.md)：実際のリポジトリにそのままコピーできるテンプレート
- [`reference/`](./reference/index.md)：手法の説明、起動手順、問題の対照表
- [`openai-advanced/`](./openai-advanced/index.md)：リポジトリ骨組み、品質文書、実行計画、システムレベルのガバナンス文書を含む、より完全な OpenAI 風の上級リソースパック

## 推奨最小構成

- `AGENTS.md` または `CLAUDE.md`
- `feature_list.json`
- `claude-progress.md`
- `init.sh`

まずこの4つをプロジェクトに入れてから agent を継続的に動かし始めると、手戻りと当て推量をかなり減らせます。

リポジトリがすでにマルチモジュール、マルチステージ、マルチロールの協働段階に入っているなら、最小テンプレートを無理に巨大で雑多なシステムへ押し広げるのではなく、[`openai-advanced/`](./openai-advanced/index.md) の上級構成にそのまま移行してください。
