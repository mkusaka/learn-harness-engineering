# Retrieval Plan

## Overview

この文書では、knowledge base application における text retrieval の実装方針をまとめます。目的は、外部の LLM API を必要とせず、取り込んだ document に対して根拠に基づく question answering を実現することです。

## Chunking Approach

document は、段落を考慮したアルゴリズムで chunk に分割されます。
- 2 つの改行（段落境界）で分割する
- chunk が約 500 文字に達するまで短い段落を結合する
- 各 chunk に一意の ID、document reference、metadata を付与する

## Keyword Matching

retrieval system は keyword-based matching を使用します。
1. question を個々の単語に tokenize する
2. stop words（3 文字未満の単語）を除外する
3. 各 chunk について、内容中に question の keyword がいくつ含まれるかを数える
4. keyword の重なり具合に基づいて chunk を順位付けする
5. 最も関連性の高い上位 2 件の chunk を citation として返す

## Citation Format

各 citation には次の情報が含まれます。
- document ID と title
- document 内での chunk index
- text excerpt（chunk の先頭 200 文字）

## Mock Q&A Patterns

mock Q&A service には、よくある topic 向けの事前定義された answer pattern が含まれています。
- architecture と design に関する question
- document の import と management
- indexing と search
- meeting notes と summaries

どの pattern にも一致しない question については、system は最も関連性の高い citation をもとに汎用的な response を返すか、indexed documents が存在しないことを示します。

## Confidence Scoring

response には confidence score が含まれます。
- citation が見つかった場合は 0.85
- citation が利用できない場合は 0.30

この scoring system により、UI は根拠のしっかりした answer と推測に基づく answer を視覚的に区別できます。
