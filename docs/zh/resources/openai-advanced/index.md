# OpenAI 高度なリソースパック

このフォルダには、OpenAI の記事「Harness engineering: leveraging Codex in an
agent-first world」で紹介されている、より完全で、より高度なリポジトリ構成をそのまま参考・複製できるテンプレートとしてまとめています。

最小限の harness では足りなくなり、リポジトリに次のような能力が必要になってきたら、このセットを確認してください。

- 短く、ルーティング責任を持つ `AGENTS.md`
- リポジトリ内に置かれた「単一の真実のソース」文書体系
- 進行中の実行計画と完了済み計画を分けて管理する仕組み
- 製品、信頼性、セキュリティ、フロントエンドのガバナンス文書
- 製品領域やアーキテクチャ層ごとに継続更新される品質スコア
- モデルが読むための参考資料ディレクトリ
- アーキテクチャ、知識の定着、運用検証のための標準 SOP

## 含まれるディレクトリ構成

[`repo-template/`](./repo-template/index.md) には、そのままコピーして使える
初期構成が用意されています。中核となるレイアウトは次のとおりです。

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

## 使い方

1. リポジトリがまだ小さいなら、まずは最小限のリソースパックを使います。
2. マルチモジュール化し、複数回の会話をまたぎ、長期的に進化する段階に入ったら、
   [`repo-template/`](./repo-template/index.md) の骨組みをリポジトリへコピーします。
3. `AGENTS.md` は短く保ち、深いルールは `docs/` に分割します。
4. 品質文書、信頼性文書、実行計画は、後から書き足すものではなく、日々の開発の一部として扱います。
5. 生成物と外部参照資料はリポジトリ内に明示的に取り込み、agent がチャット文脈や人の記憶に依存しないようにします。

## SOP ライブラリ

[`sops/`](./sops/index.md) には、記事中のいくつかの重要図を、段階的に実行できる標準手順として整理しています。

- レイヤードなドメインアーキテクチャを構築する SOP
- 見えない知識をリポジトリにコード化する SOP
- ローカルの可観測性スタックとフィードバックループの SOP
- Chrome DevTools を使って UI 検証を閉じる SOP

## 設計原則

- 短い入口、深いリンク
- リポジトリが唯一の真実のソース
- 口頭の約束より機械的な制約を優先する
- 計画、品質、技術的負債もコードと一緒にバージョン管理する
- クリーンアップと harness の簡素化は、臨時対応ではなく通常業務

このリソースパックは意図を持ったテンプレートであり、逐語的な写しではありません。最善の使い方は、高品質な出発点として取り入れ、そこから自分のプロジェクトに合わせて調整することです。
