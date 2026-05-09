[英語版 →](../../../en/projects/project-03-multi-session-continuity/)

> 関連講義: [講義 05. セッション間でコンテキストを維持する](./../../lectures/lecture-05-why-long-running-tasks-lose-continuity/index.md) · [講義 06. すべてのエージェントセッションの前に初期化する](./../../lectures/lecture-06-why-initialization-needs-its-own-phase/index.md)
> テンプレートファイル: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/en/resources/templates/)

# プロジェクト 03. セッション再開後もエージェントが作業を継続できるようにする

## やること

エージェント(agent)にスコープ制御(scope control)と検証ゲート(verification gates)を追加します。文書のチャンク化(chunking)、メタデータ抽出、インデックス作成の進捗表示、引用ベースの Q&A フローを実装します。`feature_list.json` を使って機能の状態を追跡します。一度に処理する機能は1つだけにし、検証の証跡がなければ「完了(pass)」としてマークできないようにします。

長時間実行される作業は、コンテキスト(context)の初期化や中断によって連続性(continuity)を失いやすくなります。進捗ログ(progress log)とセッションの引き継ぎを組み合わせることで、エージェントは以前に完了した項目を再確認せずに、正しい次のステップから再開できます。

2回実行します。1回目は制約なしで、2回目は厳格な適用方式で進めます。

## ツール

- Claude Code または Codex
- Git
- Node.js + Electron

## ハーネスの仕組み

進捗ログ + セッションの引き継ぎ + マルチセッション連続性(multi-session continuity)
