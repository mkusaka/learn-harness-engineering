# OpenAI Advanced Pack

このフォルダは、OpenAI の「Harness engineering: leveraging Codex in an agent-first world」記事で紹介されている、より意見色の強いリポジトリ構成を、そのまま使えるスターターファイルとしてまとめたものです。

次のような場合に、このパックを使ってください。最小構成の harness では足りなくなり、リポジトリに次のものが必要になったときです。

- ルーティング用の短い `AGENTS.md`
- リポジトリ内にある、永続的な system-of-record 文書
- 実行中と完了済みの実行計画
- 製品、信頼性、セキュリティ、フロントエンドの各ポリシーファイル
- 製品ドメインとアーキテクチャ層ごとの品質スコア
- モデルが扱いやすい参照資料フォルダ
- アーキテクチャ、知識の蓄積、実行時検証のための標準手順

## Included Starter Layout

[`repo-template/`](./repo-template/index.md) 以下のスターターパックは、次の構成を再現しています。

```text
AGENTS.md
ARCHITECTURE.md
docs/
├── design-docs/
│   ├── index.md
│   └── core-beliefs.md
├── exec-plans/
│   ├── active/
│   ├── completed/
│   └── tech-debt-tracker.md
├── generated/
│   └── db-schema.md
├── product-specs/
│   ├── index.md
│   └── new-user-onboarding.md
├── references/
│   ├── design-system-reference-llms.txt
│   ├── nixpacks-llms.txt
│   └── uv-llms.txt
├── DESIGN.md
├── FRONTEND.md
├── PLANS.md
├── PRODUCT_SENSE.md
├── QUALITY_SCORE.md
├── RELIABILITY.md
└── SECURITY.md
```

## How To Adopt It

1. リポジトリがまだ小さいなら、まずは最小構成のパックから始めてください。
2. より強い構造が必要になったら、[`repo-template/`](./repo-template/index.md) のファイルを自分のリポジトリにコピーしてください。
3. `AGENTS.md` は短く保ってください。百科事典ではなく、より深い文書への案内役として扱います。
4. quality、reliability、plan の各文書は、独立した整理日を設けるのではなく、通常の作業の一部として更新してください。
5. 生成物や外部参照は明示し、チャット履歴に頼らず agent が見つけられるようにしてください。

## SOP Library

[`sops/`](./sops/index.md) フォルダは、記事の図をステップごとの運用手順に落とし込んだものです。

- レイヤー化されたドメインアーキテクチャのセットアップ
- 未知の知識をリポジトリに組み込む
- ローカルの observability スタックとフィードバックループのワークフロー
- UI 作業のための Chrome DevTools 検証ループ

## Design Principles

- 短い入口、深くリンクされた文書
- リポジトリを system of record とする
- 覚えているルールより機械的なチェックを優先する
- 計画と品質の履歴はコードのそばに置く
- 整理と単純化は第一級の責務として扱う

このパックは意図的に意見色を持たせていますが、盲目的にコピーするのではなく、必ず自分のプロジェクトに合わせて調整してください。
