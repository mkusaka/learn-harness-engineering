# スコープの表面積の例

タスク:

- Electron のナレッジアプリに indexing を追加する

よくないスコープの切り方:

- 「indexing を実装する」

よりよいスコープの切り方:

- import されたドキュメントを parse する
- ドキュメントを chunk に分割する
- chunk の metadata を保存する
- UI に indexing の状態を表示する
- reindex アクションを追加する
