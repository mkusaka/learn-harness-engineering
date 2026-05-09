# クリーン状態チェックリスト -- Project 03

## ビルド確認

- [x] `npm install` がエラーなく完了する
- [x] `npm run check` が TypeScript エラーなしで通る
- [x] `npm run build` により `dist/` 出力が生成される

## 機能確認

- [x] ウィンドウが正しいサイズと dark theme で起動する
- [x] ドキュメント一覧に、インポート済みドキュメントが空の状態として表示される
- [x] Import ボタンでファイルピッカーが開き、`.txt` と `.md` ファイルをインポートできる
- [x] ドキュメント詳細に metadata として、title、filename、size、import date、word count、line count、file type が表示される
- [x] "View Content" ボタンでドキュメント全文を読み込み、表示できる
- [x] "Show Chunks" ボタンで chunk 化された内容と metadata を表示できる
- [x] "Index Document" ボタンで chunking が実行され、status が更新される
- [x] StatusBar が色分けされた indicator 付きで index status を表示する
- [x] StatusBar が index 済みドキュメント数と total chunk count を表示する
- [x] 質問パネルが質問を受け付け、回答を返す
- [x] 回答に、document title、chunk index、excerpt を含む citation が付く
- [x] 回答に confidence score が含まれる（citation ありは 0.85、なしは 0.30）
- [x] ドキュメントが app の再起動後も保持される
- [x] Delete ボタンでドキュメントと関連データが削除される

## スコープ管理確認

- [x] feature_list.json で全 feature が "pass" になっている
- [x] 各 feature に、実装内容を示す evidence がある
- [x] status が "fail" または "not-started" の feature がない
- [x] AGENTS.md に one-feature-at-a-time の方針が記載されている
- [x] feature の依存関係が文書化され、順守されている

## コード品質

- [x] 説明コメントのない `any` 型がない
- [x] すべての export が named export である
- [x] IPC channel は `src/shared/types.ts` でのみ定義されている
- [x] renderer が Node.js modules を import しない
- [x] service が renderer code を import しない
- [x] すべての新規ファイルが既存の conventions に従っている

## ドキュメント

- [x] docs/ARCHITECTURE.md に chunking pipeline と Q&A flow を追記した
- [x] docs/PRODUCT.md を新機能に合わせて更新した
- [x] session-handoff.md を記入した
- [x] claude-progress.md に session log がある
