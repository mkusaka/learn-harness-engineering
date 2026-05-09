# 信頼性 -- 可観測性、クリーン状態、ベンチマーク

## 構造化ログ

### 概要

アプリケーション内のすべてのサービスは、構造化された JSON ログエントリを出力します。これにより、実行時のデバッグ、事後分析、アプリケーション挙動の自動監視が可能になります。

### ログ形式

各ログエントリは 1 行の JSON オブジェクトです。

```json
{
  "timestamp": "2026-03-30T12:00:00.000Z",
  "level": "INFO",
  "service": "document-service",
  "message": "Document imported successfully",
  "data": {
    "documentId": "abc-123",
    "filename": "design-notes.md",
    "sizeBytes": 2048
  }
}
```

### ログレベル

| レベル | 使用場面 | 例 |
|-------|-------------|---------|
| DEBUG | 日常的なデータアクセス、ファイル読み込み | "Document の chunk を取得しました" |
| INFO | 重要なイベント | "Document imported", "Batch indexing complete" |
| WARN | 欠落しているが致命的ではないデータ | "Content not found for document" |
| ERROR | 失敗 | "File not found during import" |

### サービスごとのログ出力箇所

**PersistenceService:**
- ディレクトリの初期化
- ファイルの読み書き操作（DEBUG）
- クリーン状態へのリセット（WARN）

**DocumentService:**
- サイズとメタデータを伴う Document の import
- 残り件数を伴う Document の削除
- Document メタデータの更新
- ファイルが見つからないエラー
- サイズ制限違反

**IndexingService:**
- 単一およびバッチ indexing の開始
- Document ごとの indexing 進捗
- スループット指標を伴うバッチ完了
- Content が見つからない警告

**QaService:**
- 質問処理の開始
- confidence と duration を伴う回答生成
- フィードバック送信
- 履歴の消去

**IPC Handlers:**
- すべてのチャネル呼び出し（変更系は INFO、読み取り系は DEBUG）
- 起動時に登録されているすべてのチャネル

### ログレベルの設定

`LOG_LEVEL` 環境変数を設定します。
```bash
LOG_LEVEL=INFO npm run dev  # INFO, WARN, ERROR のみ
LOG_LEVEL=WARN npm run dev  # WARN と ERROR のみ
LOG_LEVEL=ERROR npm run dev # ERROR のみ
```

デフォルト: `DEBUG`（すべてのメッセージ）。

## クリーン状態の管理

### 目的

クリーン状態の管理は、テストとベンチマークを既知の空の状態から開始できるようにします。これにより、蓄積したデータがテスト結果に影響したり、予期しない挙動を引き起こしたりするのを防げます。

### リセットの仕組み

アプリケーションには、次の処理を行う `RESET_DATA` IPC チャネルがあります。

1. データディレクトリ全体（`knowledge-base-data/`）を削除する
2. ディレクトリ構造を再作成する
3. 成功レスポンスを返す
4. renderer 側で React の state をすべてクリアして再読み込みする

### クリーン状態を使う場面

- ベンチマークを実行する前
- デバッグ作業の後
- 新機能をテストする前
- データディレクトリが破損したとき

### クリーン状態の確認

`clean-state-checklist.md` を使って、次を確認します。
- Build passes without errors
- アーキテクチャの境界が守られている
- 実行時の挙動が正しい
- ログ出力が期待どおりである
- データの整合性が維持されている

## ベンチマーク

### 概要

`scripts/benchmark.sh` スクリプトは、主要な操作にわたってアプリケーションの性能を測定します。Electron ウィンドウを起動せず、ファイルベースのシミュレーションで services 層をテストします。

### ベンチマーク対象

| タスク | 測定内容 | 目標 |
|------|------------------|--------|
| `import` | Document import のスループット | 3 ファイルを 1 秒未満 |
| `index` | バッチ indexing 速度 | 14 chunks を 1 秒未満 |
| `query` | Q&A の応答レイテンシ | 1 問あたり 500ms 未満 |
| `verify` | データ整合性チェック | エラー 0 件 |

### ベンチマークの実行

```bash
bash scripts/benchmark.sh
```

出力例:
```
=== Benchmark Results ===
[import] 3 files: 120ms (25.0 files/sec)
[index]  3 documents: 80ms (175.0 chunks/sec)
[query]  5 questions: 1250ms (250.0ms avg)
[verify] Data integrity: PASS
=== Summary: 4/4 tasks passed ===
```

### 結果の見方

- import が遅い場合は、ファイルサイズとディスク I/O を確認します。
- indexing が遅い場合は、chunk サイズと段落の境界を確認します。
- query が遅い場合は、chunk 数とキーワード一致を確認します。
- verify に失敗した場合は、`scripts/cleanup-scanner.sh` を実行して問題箇所を特定します。

## クリーンアップスキャナ

### 概要

`scripts/cleanup-scanner.sh` スクリプトは、古い、または不整合な成果物がないかデータディレクトリを確認します。

### チェック内容

| チェック | 説明 |
|-------|-------------|
| 孤立した content ファイル | メタデータに対応する document がない content ファイル |
| ぶら下がった chunk ファイル | 対応する index エントリがない chunk ファイル |
| 不足している content ファイル | content ファイルがないメタデータ上の document |
| 不整合なメタデータ | chunk ファイルがないのに indexed とマークされている document |
| 空のデータファイル | 本来データがあるはずなのに空配列になっている JSON ファイル |
| 古い Q&A 履歴 | 削除済み document を参照している履歴エントリ |

### スキャナの実行

```bash
bash scripts/cleanup-scanner.sh
```

出力例:
```
=== Cleanup Scanner ===
[OK] No orphaned content files
[OK] No dangling chunk files
[OK] No missing content files
[OK] All indexed documents have chunk files
[OK] No stale Q&A references
=== Result: CLEAN (0 issues) ===
```
