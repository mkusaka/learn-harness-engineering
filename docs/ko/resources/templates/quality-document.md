# 品質ドキュメント (Quality Document)

各プロダクトドメイン(product domain)とアーキテクチャ(architecture)レイヤ(layer)の品質スナップショット(snapshot)です。エージェント(agent)と人間の両方がこの文書を使って、コードベース(codebase)の強みと補強が必要な部分をすばやく把握できます。

この文書は、個々のセッションの成果物ではなく、プロジェクト全体の健全性を時間の経過とともに追跡するという点で、評価者ルーブリック(evaluator rubric)と区別されます。

**更新頻度:** 重要なセッションのたび、または新しい作業段階に入る前。

**等級基準:**

- **A**: すべての検証(verification)に合格、クリーン(clean)なアーキテクチャ、エージェントから読み取り可能(agent-legible)、安定したテスト
- **B**: 検証に合格、概ねクリーン、読みやすさまたはテストカバレッジに小さな不足あり
- **C**: 部分的に動作、既知の不足あり、一部のコード領域がエージェントにとって理解しづらい
- **D**: 動作しない、または主要な構造上の問題がある

---

## プロダクトドメイン (Product Domains)

| ドメイン (Domain) | 等級 (Grade) | 検証 (Verification) | エージェント可読性 (Agent Legibility) | テスト安定性 (Test Stability) | 主な不足 (Key Gaps) | 最終更新 (Last Updated) |
|--------|-------|-------------|-----------------|---------------|----------|-------------|
| Document Import | - | - | - | - | - | - |
| Document Management | - | - | - | - | - | - |
| Document Indexing | - | - | - | - | - | - |
| Q&A Flow | - | - | - | - | - | - |
| Grounded Answers | - | - | - | - | - | - |

## アーキテクチャレイヤ (Architectural Layers)

| レイヤ (Layer) | 等級 (Grade) | 境界の適用 (Boundary Enforcement) | エージェント可読性 (Agent Legibility) | 主な不足 (Key Gaps) | 最終更新 (Last Updated) |
|-------|-------|---------------------|-----------------|----------|-------------|
| Main Process | - | - | - | - | - |
| Preload | - | - | - | - | - |
| Renderer | - | - | - | - | - |
| Services | - | - | - | - | - |

## 変更履歴 (Change History)

### YYYY-MM-DD

- 変更:
- 昇格したドメイン:
- 降格:
- 新たに見つかった不足:
- 解消した不足:
