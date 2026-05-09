# OpenAI Advanced Pack

このフォルダは、OpenAI の記事「Harness engineering: leveraging Codex in an agent-first world」で説明されている、より洗練されたリポジトリ構成を、そのままコピーできるスターターファイル群としてまとめたものです。

このパックは、最小限の harness では足りなくなり、リポジトリに次のようなものが必要になったときに使ってください。

- ルーター役として機能する短い `AGENTS.md`
- リポジトリ内のシステム・オブ・レコードとしての長期的なドキュメント
- 実行中と完了済みの exec plans
- プロダクト、信頼性、セキュリティ、フロントエンドに関する明示的なポリシーファイル
- プロダクト領域やアーキテクチャ層ごとの品質スコアリング
- モデルにとって扱いやすい参照資料フォルダ
- アーキテクチャ、知識の定着、ランタイムでの検証に関する標準運用手順

## スターター構成の内容

[`repo-template/`](./repo-template/index.md) にあるスターターパックは、以下の構成を再現しています。

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

1. リポジトリがまだ小さいなら、まずは最小構成のパックから始めてください。
2. より厳密な構造が必要になったら、[`repo-template/`](./repo-template/index.md) からファイルをリポジトリへコピーしてください。
3. `AGENTS.md` は短く保ってください。百科事典ではなく、より深いドキュメントへ誘導するルーターとして扱ってください。
4. 品質、信頼性、計画に関するドキュメントは、片付けのための特別な日ではなく、通常業務の中で更新してください。
5. 生成物や外部リンクは明示して、エージェントがチャット履歴に頼らず見つけられるようにしてください。

## SOP ライブラリ

[`sops/`](./sops/index.md) フォルダは、記事中の図を段階的な運用手順に変換します。

- レイヤードなドメインアーキテクチャのセットアップ
- 暗黙知をリポジトリに組み込む方法
- ローカルの observability スタックとフィードバックループのワークフロー
- Chrome DevTools を使った UI 検証のサイクル

## 設計原則

- 短い入口から、より深い関連ドキュメントへつなぐ
- リポジトリをシステム・オブ・レコードとして扱う
- 覚えたルールより、機械的なチェックを優先する
- 計画と品質の履歴はコードの近くに置く
- 整理と簡素化は第一級の責務にする

このパックは意図的に精査されていますが、それでも無条件に丸ごとコピーするのではなく、必ず自分のプロジェクトに合わせて調整してください。
