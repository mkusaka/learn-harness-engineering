# OpenAI 高度パック(Advanced Pack)

このフォルダは、OpenAI の "Harness engineering: leveraging Codex in an agent-first world" で説明されている、より堅牢な repository 構成をそのまま使えるスターターファイルとしてまとめたものです。

基本的な harness だけでは不十分で、次のようなものが必要になったときにこのパックを使ってください。

- ルーティング用の簡潔な `AGENTS.md`
- リポジトリ内で永続的な system-of-record 文書
- 有効中と完了済みの execution plan
- product、reliability、security、frontend の明示的なポリシーファイル
- 製品ドメインとアーキテクチャ層ごとの品質スコア管理
- agent に優しい参考資料フォルダ
- アーキテクチャ、knowledge capture、runtime 検証のための標準作業手順(SOP)

## 含まれるスターターレイアウト

[`repo-template/`](./repo-template/index.md) 配下のスターターパックは、次の構成を反映しています。

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

## 導入方法

1. リポジトリがまだ小さいなら、まず minimal pack から始めてください。
2. さらに強い構成が必要になったら、[`repo-template/`](./repo-template/index.md) のファイルを自分のリポジトリにコピーしてください。
3. `AGENTS.md` は短く保ってください。このファイルは詳細な文書へのルーターとして扱い、百科事典のようには使わないでください。
4. quality、reliability、plan の文書は、特別な整理日を設けず、日常作業の一部として更新してください。
5. 生成済みの artifact と外部参考資料を明示的に管理し、agent がチャット履歴に頼らず見つけられるようにしてください。

## SOP ライブラリ

[`sops/`](./sops/index.md) フォルダは、原文の図解を段階的な運用手順(standard operating procedure, SOP)に変換したものです。

- layered domain architecture の設定
- 暗黙の知識をリポジトリにエンコードすること
- ローカル observability スタックと feedback loop のワークフロー
- UI 作業のための Chrome DevTools 検証ループ

## 設計原則

- 短い入口と、そこから深くつながる文書
- system-of-record としてのリポジトリ
- 機械的な検査は、記憶に頼るルールより優れている
- plan と quality の履歴はコードの隣に置く
- 整理と単純化は first-class の責務である

このパックには意図的に見解が含まれていますが、それでも盲目的にコピーせず、自分のプロジェクトに合わせて調整して使うべきです。
