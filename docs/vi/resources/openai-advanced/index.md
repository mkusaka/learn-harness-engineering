[英語版 →](../../../en/resources/openai-advanced/) | [中国語版 →](../../../zh/resources/openai-advanced/)

# OpenAI の高度な Harness パッケージ

このパッケージは、OpenAI の記事「Harness Engineering」で説明されている harness の設計を、実用的に使えるスターター一式と、それに付随する SOP 構造としてまとめたものです。

## なぜ存在するのか

Harness Engineering の記事では、リポジトリは記録システムであること、外部化された記憶、記憶ではなく機械的な検証、そして回復ループといった高レベルの原則が説明されています。このパッケージは、それらの原則を次の形に落とし込みます。

- 実際の repo に向けた、明確に構造化された文書一式
- プロダクト領域とアーキテクチャ層に応じた品質スコアリング
- model にとって扱いやすい参照ドキュメント用ディレクトリ
- アーキテクチャ、知識の取り込み、runtime 検証のための標準運用手順

## すぐ使える構成

[`repo-template/`](./repo-template/index.md) にあるスターターパッケージは、次の構成を反映しています。

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

## 適用方法

1. repo がまだ小さいうちは、最小構成のパッケージから始めてください。
2. より強い構造が必要になったら、[`repo-template/`](./repo-template/index.md) 内のファイルを自分のリポジトリへコピーしてください。
3. `AGENTS.md` は短く保ってください。百科事典ではなく、より深い文書へのルーターとして扱ってください。
4. 品質、信頼性、計画に関する文書は、特別な片付けの日にまとめて更新するのではなく、日々の作業の一部として更新してください。
5. 生成された artifact と外部参照は明確に保ち、agent がチャット履歴に頼らず見つけられるようにしてください。

## SOP ライブラリ

[`sops/`](./sops/index.md) ディレクトリは、記事内の図を段階的な運用手順に変換します。

- 分層ドメインアーキテクチャの構築
- 暗黙知をリポジトリに定着させる
- ローカル observability スタックとフィードバックループのワークフロー
- UI 作業のための Chrome DevTools 検証ループ

## 設計原則

- 入り口は短くし、深い内容はリンク先に持たせる
- リポジトリは記録システムである
- 機械的な検証は、記憶に頼った規則より優れている
- 計画と品質履歴はコードのそばに置く
- クリーンアップと単純化は最優先の責務である

このパッケージは意図的に強い主張を持っていますが、盲目的にコピーするのではなく、自分のプロジェクトに合わせて調整してください。
