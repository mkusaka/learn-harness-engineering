# プロンプト調整 (Prompt Calibration)

ルート指示(root instructions)は、可能なすべての行動を列挙するのではなく、運用フレーム(operating frame)を定義するべきです。ファイルが大きくなりすぎると、エージェント(agent)が重要な情報を見つけにくくなり、保守もしづらくなります。

## ルートファイルに残す内容 (Keep In The Root File)

- リポジトリの目的と範囲
- 起動パス(startup path)
- 検証(verification)パス
- 妥協できない制約(non-negotiable constraints)
- 必須の状態成果物(required state artifacts)
- セッション終了ルール

## ルートファイルから移す内容 (Move Out Of The Root File)

- 古い履歴(history)のエッジケース(edge cases)
- トピックごとの実装詳細
- コードの近くに置くべきローカルアーキテクチャ(architecture)ノート
- 1つのサブシステム(subsystem)にのみ適用される例

## 作業ルール (Working Rule)

ルートファイルは、新しいセッションがすばやく方向をつかめるようにするためのものです。ファイルが過去のあらゆる失敗例をダンプ(dumping)する場所になっているなら、詳細はより小さな文書に分割し、代わりにリンクを使ってください。
