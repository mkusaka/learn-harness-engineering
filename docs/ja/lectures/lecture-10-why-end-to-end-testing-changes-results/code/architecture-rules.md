# Electron Architecture Rules

- Renderer コードは filesystem に直接アクセスしてはならない。
- Preload は renderer と Electron main をつなぐ唯一の bridge である。
- retrieval と indexing のロジックは UI コンポーネントではなく service モジュールに置く。
- Logging は構造化し、service boundary から出力するべきである。
