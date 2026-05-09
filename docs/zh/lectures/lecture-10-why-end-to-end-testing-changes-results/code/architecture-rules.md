# Electron のアーキテクチャルール

- renderer のコードはファイルシステムへ直接アクセスしてはならない。
- renderer と Electron main をつなぐ唯一の橋渡しは preload である。
- 取得とインデックス作成のロジックは UI コンポーネントではなく、service モジュールに置く。
- ロギングは構造化し、service 境界から出力すること。
