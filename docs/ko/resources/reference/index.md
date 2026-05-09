# 参考資料 (Reference)

このノートでは、テンプレート(template)の集まりを単なるファイルの寄せ集めではなく、実際に動作するハーネス(harness)として使う方法を説明します。各文書は特定の失敗タイプ(failure mode)を扱っており、あわせて読むことで、安定した長期エージェント(agent)作業環境を構築する全体像をつかめます。

## 参考ノート (Reference Notes)

- [`method-map.md`](./method-map.md): 長時間実行中によく発生する失敗タイプを、その問題を最初に解決する成果物(artifact)またはポリシー(policy)に対応付けます。
- [`initializer-agent-playbook.md`](./initializer-agent-playbook.md): 機能作業が始まる前に初期化エージェントが残しておくべき内容。
- [`coding-agent-startup-flow.md`](./coding-agent-startup-flow.md): 以後のコーディングセッションに向けた固定のセッション開始フロー。
- [`prompt-calibration.md`](./prompt-calibration.md): ルート指示を肥大化させたり脆くしたりせずに、鋭さを保つ方法。

## 推奨の読み順 (Suggested Reading Order)

1. `method-map.md`
2. `initializer-agent-playbook.md`
3. `coding-agent-startup-flow.md`
4. `prompt-calibration.md`
