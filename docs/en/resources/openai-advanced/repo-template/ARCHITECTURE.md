# ARCHITECTURE.md

このファイルはシステム全体の最上位マップです。簡潔に保ち、必要に応じて
より詳細な文書へ案内してください。

## System Shape

- Product: `[replace with product name]`
- 主要なユーザーワークフロー: `[replace with main workflow]`
- 実行面: `[desktop / web / cli / services / workers]`
- 製品挙動の唯一の正本: `docs/product-specs/`

## Domain Map

| Domain | Purpose | Primary Entry Points | Related Spec |
|--------|---------|----------------------|--------------|
| `[domain-a]` | `[what it owns]` | `[modules / routes / commands]` | `[spec path]` |
| `[domain-b]` | `[what it owns]` | `[modules / routes / commands]` | `[spec path]` |

## Layer Model

エージェントが場当たり的なアーキテクチャを作らないよう、固定された
方向性モデルを使ってください。

`Types -> Config -> Repo -> Service -> Runtime -> UI`

横断的な関心事は、レイヤーを直接またいで参照するのではなく、明示的な
provider または adapter の境界を通して取り込んでください。

## Hard Dependency Rules

- 下位レイヤーは上位レイヤーに依存してはいけません。
- UI は runtime や service の契約を迂回してはいけません。
- データアクセスは repositories、または同等の adapter を通して行ってください。
- 共通ユーティリティは汎用的なまま保ち、ドメインロジックを溜め込んではいけません。
- 新しい依存関係は、対応する plan か design doc で根拠を示してください。

## Cross-Cutting Interfaces

| Concern | Approved Boundary | Notes |
|--------|-------------------|-------|
| Logging and tracing | `[provider / utility path]` | `[structured only, no ad hoc console use]` |
| Auth | `[provider path]` | `[token/session rules]` |
| External APIs | `[client or provider path]` | `[rate limit / retry guidance]` |
| Feature flags | `[flag boundary]` | `[ownership]` |

## Current Hot Spots

- `[agents が安全に変更しにくい領域]`
- `[境界が弱い、またはテストが脆弱な領域]`

## Change Checklist

アーキテクチャに関わるコードを変更したら:

1. ドメインマップまたは許可された境界が変わった場合は、このファイルを更新してください。
2. 根拠が変わった場合は、`docs/design-docs/` の関連 design doc を更新してください。
3. ルールを機械的に強制すべきなら、実行可能なチェックを追加または更新してください。
