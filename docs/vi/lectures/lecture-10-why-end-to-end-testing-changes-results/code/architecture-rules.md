# Electron のアーキテクチャ規則

- renderer のコードはファイルシステムに直接アクセスしてはいけない。
- Preload は renderer と Electron main の間にある唯一の橋渡しである。
- 取得と indexing のロジックは UI component ではなく、service module に置く。
- Logging は構造化し、service の境界から出力する。
