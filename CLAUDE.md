# CLAUDE.md

このファイルは、このリポジトリでコードを扱う際の Claude Code (claude.ai/code) 向けガイドです。

## プロジェクト概要

Learn Harness Engineering は、AI エージェント向けの信頼性の高いコーディング環境を構築するための、プロジェクトベースの講座です。このリポジトリには、VitePress のドキュメントサイトと実践用のプロジェクトコードが含まれています。

## コマンド

```sh
# ドキュメントサイト
npm install
npm run docs:dev        # ホットリロード付きの開発サーバー (VitePress)
npm run docs:build      # 本番ビルド
npm run docs:preview    # ビルド済みサイトのプレビュー

# 講義のコード例を実行
npx tsx docs/lectures/<lecture-dir>/code/<file>.ts

# プロジェクトの Electron アプリ（各プロジェクトディレクトリから）
cd projects/project-NN/starter  # or solution/
npm install
npm run dev              # ビルドして Electron を起動 (scripts/dev.js 経由)
npm run check            # tsconfig.json と tsconfig.node.json の両方を型チェック
npm run test             # Vitest を 1 回実行
npm run test:watch       # Vitest の watch モード
```

## リポジトリ構成

- `docs/` — VitePress のドキュメントサイト (lectures, projects, resources)
- `docs/.vitepress/config.mts` — EN/ZH 両ロケール向けのナビゲーション / サイドバー設定
- `docs/lectures/` — 12 の講義。各講義に `index.md` と `code/` の例を含む
- `docs/projects/` — 6 つのプロジェクト説明
- `docs/resources/` — 英中バイリンガルのテンプレート、参考資料、OpenAI advanced pack
- `projects/shared/` — 共有の Electron + TypeScript + React 基盤
- `projects/project-NN/` — 各プロジェクトごとの `starter/` と `solution/` ディレクトリ

## アーキテクチャ

講座は、6 つのプロジェクトを通じて進化していく Electron ベースのナレッジベースデスクトップアプリを中心に構成されています:
- **Main process** (`src/main/`): ウィンドウ管理、IPC ハンドラー、サービス初期化
- **Preload** (`src/preload/`): 型付き API を renderer に公開する contextBridge
- **Renderer** (`src/renderer/`): ドキュメント一覧、Q&A パネル、ステータスバーを備えた React UI
- **Services** (`src/services/`): DocumentService、IndexingService、QaService、PersistenceService
- **Shared types** (`src/shared/types.ts`): 境界をまたぐインターフェースと IPC チャンネル定数

各プロジェクトの `starter/solution` は、その進化段階における Electron アプリの完全なコピーです。P(N+1) の starter は P(N) の solution から派生します。共通基盤は `projects/shared/` にあります。

## 主要パターン

- IPC チャンネルは `src/shared/types.ts` の定数 (`IPC_CHANNELS`) として定義し、単一の信頼できる情報源にする
- すべてのデータは JSON / テキストファイルとしてローカル保存する（データベースは使わない）
- モック Q&A は、引用付きの構造化された回答を返す（実際の LLM API は使わない）
- プロジェクトルートのハーネスファイル: AGENTS.md, CLAUDE.md, feature_list.json, init.sh, claude-progress.md
- 段階的開示: 短い AGENTS.md を起点に、必要な詳細ドキュメントへリンクする
- 各プロジェクトには 2 つの tsconfig がある: `tsconfig.json` (renderer) と `tsconfig.node.json` (main/preload)

## バイリンガルコンテンツ

すべてのコンテンツは英語と中国語の両方で存在します。ドキュメントは共有の `docs/lectures/` と `docs/projects/` ディレクトリにあります（各ファイル内でバイリンガルになっています）。リソースは `docs/resources/en/` と `docs/resources/zh/` に分かれています。両者を同期した状態に保ってください。
