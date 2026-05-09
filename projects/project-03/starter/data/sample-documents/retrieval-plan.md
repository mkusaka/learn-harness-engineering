# Retrieval Plan

## Overview

この文書では、ナレッジベースアプリケーションにおけるテキスト検索機能の実装方針をまとめます。目的は、外部の LLM API を使わずに、取り込んだ文書に基づく根拠付きの質問応答を可能にすることです。

## Chunking Approach

文書は、段落を意識したアルゴリズムで chunk に分割します。
- 2 つの改行（段落の境界）で分割する
- 短い段落は、chunk が約 500 文字に達するまで結合する
- 各 chunk には一意の ID、文書参照、metadata を付与する

## Keyword Matching

retrieval system は、キーワードベースのマッチングを使います。
1. 質問を個々の単語に tokenization する
2. stop words（3 文字未満の単語）を除外する
3. 各 chunk について、質問のキーワードが content にどれだけ含まれているかを数える
4. キーワードの重なり具合のスコアで chunk を順位付けする
5. 最も関連性の高い上位 2 件の chunk を citation として返す

## Citation Format

各 citation には次の情報を含めます。
- Document ID と title
- 文書内での chunk index
- テキストの抜粋（chunk の先頭 200 文字）

## Mock Q&A Patterns

mock Q&A service には、よくあるトピック向けの定義済み回答パターンが含まれます。
- Architecture and design 質問
- Document の import と management
- Indexing と search
- Meeting notes と summaries

パターンに一致しない質問については、システムは最も関連性の高い citation に基づく一般的な応答を返すか、indexed documents が利用できないことを示します。

## Confidence Scoring

応答には confidence score を含めます。
- citation が見つかった場合は 0.85
- citation がない場合は 0.30

このスコアリングにより、UI で根拠のある回答と推測に基づく回答を視覚的に区別できます。
