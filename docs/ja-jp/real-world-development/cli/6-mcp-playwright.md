---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI の Playwright MCP を確認または設定し、ブラウザーでフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

すでに確認したように、コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が追加の自動化を利用できるようにすることも必要です。そこで MCP サーバーを使います。MCP サーバーを利用すると、Copilot は CLI に組み込まれていないツールやサービスも使えるようになります。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを学びます。
- Playwright MCP サーバーがまだ利用できない場合は追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を試します。

## シナリオ

ユニット テストとエンドツーエンド テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。ユーザーと同じように Copilot が作業中の Web サイトを使えるようにすると、変更が期待どおりに動作するか、さらに自動的に確認できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための方法を提供します。MCP を使うと、AI エージェントは外部サービスとリアルタイムに通信し、最新の情報にアクセスしたり、依頼に応じて操作したりできます。

これらのツールやリソースには、AI エージェントと外部サービスをつなぐ MCP サーバーを通じてアクセスします。MCP サーバーごとに、エージェントが利用できるツールやリソースのセットが異なります。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化機能を利用できます。

他にも多くの MCP サーバーがあります。GitHub は、エコシステム内での発見や貢献を促進するために [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーも、プロジェクトの他の依存関係と同様に扱ってください。使用する前にソース コードをレビューし、公開元を確認して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は追加し、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI に Playwright MCP がすでに設定されている場合は、セットアップ手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類が **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを終了します。

## Playwright で機能を試すよう Copilot に依頼する

これまでは機能が期待どおりに動作するか自分で確認しました。今度は Copilot で利用できる Playwright MCP サーバーを使って、同じ確認を行いましょう。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に伝えます。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は MCP サーバーを使うよう Copilot に指定しなくても、Copilot が自動的に判断します。ただし、使用するツールが分かっている場合は、明示すると結果の一貫性を高め、トークンを節約できます。

2. ブラウザーで機能を確認するため、Copilot が実行する手順を確認します。
3. 結果の報告を読み、すべてが期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作して、サーバーを停止した後、報告を作成します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認できました。ここでは、次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを学びました。
- Playwright MCP サーバーがまだ利用できない場合に追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を試しました。

次は、スキルとブラウザー ツールを専門的な役割にまとめる [QA カスタム エージェントを作成し][next-lesson]ます。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
