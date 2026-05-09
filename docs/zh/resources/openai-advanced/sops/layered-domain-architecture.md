# SOP：分層ドメインアーキテクチャ

agent が層をまたいで無秩序につなげ続けたり、別の層で同じロジックを重複させたり、数回の会話を経るうちにコードがどんどんレビューしづらくなったりするなら、この SOP を使います。

## 目的

ドメイン境界を明確に書き、確立し、実行可能にして、agent が高速に動いても、気づかないうちに構造を壊さないようにします。

## 目標モデル

1 つの業務ドメインの内部では、まず次の一方向の流れを使います。

`Types -> Config -> Repo -> Service -> Runtime -> UI`

ドメインをまたぐ関心事は、明示的な provider か adapter を通して入れます。共有 utils はドメインの外に置き、業務ロジックのごみ箱にじわじわ変質させてはいけません。

## 構築チェックリスト

- `ARCHITECTURE.md` に現在の domains を列挙する。
- `ARCHITECTURE.md` に許可される依存方向を明記する。
- auth、telemetry、外部 API などの cross-cutting interface を記録する。
- 現時点で最も扱いにくい境界違反について、短い説明を書く。
- どのルールを lint、test、script に昇格させるべきか決める。

## 実行 SOP

1. まずコードベースの domain map を作ってから、実装スタイルを考える。
2. 各 domain ごとに、許可する layer sequence を明確にする。
3. すべての横断的関心事を洗い出し、provider か adapter に切り替える。
4. あいまいな shared logic は、所属する domain に戻すか、本当に汎用の utils として切り出す。
5. ルールを `ARCHITECTURE.md` に書き込む。
6. まず最もコストの高い種類の違反に対して、実行可能な guardrail を 1 つ追加する。
7. 修正後に quality score を更新する。

## 完了条件

- 新しく入った agent でも、どの変更をどの層に置くべきか判断できる。
- UI が repo や外部副作用に直接つながらない。
- 横断的な機能に明確な入口がある。
- 少なくとも 1 つの重要な境界が機械的に強制されている。

## 同期更新が必要なリポジトリ成果物

- `ARCHITECTURE.md`
- `docs/QUALITY_SCORE.md`
- 設計理由が変わった場合は `docs/design-docs/` を更新する
- `docs/PLANS.md` または現在の active execution plan
