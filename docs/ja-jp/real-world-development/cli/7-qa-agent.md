---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルタリングを確認しました。次は、明確な QA プロセスを定義したカスタム エージェントに、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選び、フィルタリングの issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタム エージェントを使うと、この QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する専門化された Copilot です。プロファイルには、エージェントの目的、指示、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明します。`quality-checks` スキルは繰り返し実行するチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、要件を評価して結果を報告するために Copilot がこれらの機能をどう使うかを指示します。既存の機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、Copilot に再利用可能な QA プロファイルを作成してもらいます。プロファイルでは、QA で実行するチェックと、守るべき境界の両方を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、新しいカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、意図した QA ワークフローと境界が Copilot に正しく反映されているか、プロファイルをレビューします。これにより、確認だけが必要なときに、エージェントが不完全な指示や広すぎる権限で機能を変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須です。`name` は任意ですが、含めるとエージェントの表示名が明確になります。
3. プロファイルの指示を読み、QA が要件を起点とし、リポジトリの指示に従い、`quality-checks` スキルを実行して Playwright MCP を利用することを確認します。
4. QA が根拠を示し、実装コードを変更する前に許可を求め、変更をコミットしたりプルリクエストを作成したりしないことを確認します。
5. これらの役割や境界が抜けている場合は、続行する前に既定のエージェントに修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成したら同じフィルタリングの会話を再開し、QA を選択します。そうすると、issue と計画で決めた内容を利用できます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントは、利用可能なエージェントの一覧にまだ表示されないことがあります。その場合でも、上記のコマンドで有効にできます。

2. 次のプロンプトで機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、レポートを確認します。

## まとめと次のステップ

再利用可能な専門的役割をワークフローに加え、結果をレビューしました。このレッスンでは、次のことを行いました。

- カスタム エージェントが指示、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選び、フィルタリングの issue に照らして結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートがそろいました。次は、それらをまとめて[機能 PR を作成し、Agent Merge を使い][next-lesson]ます。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
