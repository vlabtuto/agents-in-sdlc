---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキスト管理、モデル選択、セッション共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

他の CLI ツールと同様に、GitHub Copilot CLI にも多くのスラッシュ コマンドがあります。これらのコマンドを使うと、高度な機能、内部情報、追加の構成オプションを利用できます。これまで `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、さらに役立つコマンドをいくつか確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の基本的なワークフローを終えました。ここでは、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組んでいると、モデルのコンテキスト ウィンドウの上限に近づくことがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュ コマンドを使って自分で確認したり圧縮したりできます。

1. Codespace に戻り、まだ起動していない場合は、リポジトリのルートから Copilot CLI を起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分けられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を実行して結果を比較します。会話がすでに短い場合は、大きな違いが見られないことがあります。

> [!NOTE]
> コンテキスト ウィンドウがいっぱいになると、Copilot CLI は自動的にコンテキストを圧縮します。タイミングを自分で決めたいときは `/compact` を使います。関連のないタスクに切り替えて新しい会話を始めたい場合は、代わりに `/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも違います。Copilot CLI では、利用できるモデルを表示して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを使うか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、AI ツールをより活用できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を実行し、インストール済みのバージョンで利用できる `/share` のオプションを確認します。
2. セッションを共有する場合は、ローカルに Markdown としてエクスポートする `/share file` など、目的に合う方法を選びます。
3. エクスポートした内容を、誰かに送ったり公開したりする前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容を公開しないでください。

## 任意: 作業を並行化または委任する

ワークショップの主要な内容は完了しました。Copilot CLI には、大規模なタスクで使える次のコマンドもあります。

- `/fleet` は、独立したサブタスクに分割し、サブエージェントを並列で実行できます。
- `/worktree` は、別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は、非同期で作業してプルリクエストを作成することがある Copilot cloud agent にタスクを送信できます。

これらのコマンドは追加の worktree やリモート作業を作成することがあるため、任意です。試すときは、新しく範囲を明確にしたタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく確認するには、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、構成を変更したり、セッションを共有したり、内部の状況を確認したりできます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI には、まだ多くのスラッシュ コマンドがあります。次は、学んだ内容と今後の学習方法を[振り返り][next-lesson]ましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
