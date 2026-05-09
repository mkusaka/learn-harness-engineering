[English 版 →](../../../en/projects/project-01-baseline-vs-minimal-harness/)

> 関連講義: [講義 01. 強力なモデルだからといって、信頼できる実行になるとは限りません](./../../lectures/lecture-01-why-capable-agents-still-fail/index.md) · [講義 02. ハーネスとは実際には何か](./../../lectures/lecture-02-what-a-harness-actually-is/index.md)
> テンプレートファイル: [templates/](https://github.com/walkinglabs/learn-harness-engineering/blob/main/docs/en/resources/templates/)

# プロジェクト 01. プロンプト単独 vs. ルール先行(Rules-First): どれほど差があるのか

## やること

最小限の Electron ベースのナレッジベースアプリのシェル(shell)を構築します。左側にドキュメント一覧、右側に Q&A パネルがあり、ローカルデータディレクトリを含むウィンドウです。作業そのものは複雑ではありません。複雑なのは、エージェント(agent)にこの作業を完了させる方法です。

2 回実行します。1 回目は、プロンプトだけを使い、事前準備なしで進めます。2 回目は、`AGENTS.md`、`init.sh`、`feature_list.json` をリポジトリにあらかじめ配置した状態で進めます。そのうえで比較します。

このプロジェクトの核心は、コードを書くことではありません。「ルールを先に整えるために 15 分使うこと」と「エージェントをそのまま実行すること」の間に、どれほど大きな差があるかを確かめることです。

エージェントにルール(rule)と初期化(initialization)の手がかりを与えると、作業範囲を自分で把握し、不要な探索を減らせます。このような最小限のハーネスだけでも、エージェントの立ち上がり方と成果物の品質は大きく変わります。

## ツール

- Claude Code または Codex（どちらか 1 つを選び、2 回の実行で同じものを使う）
- Git（ブランチ管理と比較）
- Node.js + Electron（プロジェクトのスタック）
- タイマー（各実行の所要時間を記録）

## ハーネスメカニズム

最小ハーネス(minimal harness): `AGENTS.md` + `init.sh` + `feature_list.json`
