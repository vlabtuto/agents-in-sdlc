---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent Mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

Agent Mode を使ってフィルタリング機能を作成しました。プルリクエストを作成する前に、ブラウザーで実際に動作することを確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開してプルリクエストを作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot を拡張する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーでフィルタリング機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントがエディターの外部にアクセスできると、Agent Mode はさらに強力になります。Model Context Protocol (MCP) は Copilot が外部にアクセスするための標準であり、エージェントが外部のツールやサービスと通信する方法です。

![Agent Mode の内部と、コンテキスト、LLM、ツール (MCP サーバーや VS Code 拡張機能が提供するツールを含む) との連携を示す図。](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP によって AI エージェントは外部のツールやサービスとリアルタイムにやり取りできるため、リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作を実行したりできます。

これらのツールとリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。MCP サーバーは、AI エージェントと外部ツール (既存の API や NPM パッケージなどのローカル ツール) 間の通信を管理します。各 MCP サーバーは、AI エージェントが利用できるツールとリソースのセットを提供します。

よく使われている MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する一連の API にアクセスできます。AI エージェントは、新しいリポジトリの作成、既存リポジトリの更新、課題やプルリクエストの管理などを行えます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントは、Web ページへの移動、フォームへの入力、ボタンの選択などを行えます。

ほかにも、さまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーの検索やコントリビューションを支援する [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点では、MCP サーバーをほかの依存関係と同様に扱ってください。使用する前に、ソース コードと発行元を慎重に確認し、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使用し、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot が課題のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルへのインストールは不要です。VS Code は HTTP 経由で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと Copilot が制御できるブラウザーを利用できます。今回の機能をテストするのに最適です。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、ファイルが次のようになるようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモート サーバーの `github` と違い、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、Playwright はウィンドウを表示せずにブラウザーを実行します。デスクトップ環境がない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンド テストに Playwright をすでに使っているため、通常、必要なブラウザーはインストール済みです。後で Copilot からブラウザーがないと報告された場合は、`npx playwright install chromium` を実行して再試行するよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がサーバーのツールを初めて使うときに、VS Code がサーバーを起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、構成を確認し、信頼することを選択してサーバーを起動します。

手動での起動は省略できます。その場合、次の手順で Copilot がツールを初めて使うときに VS Code がサーバーを起動し、信頼するか確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。アプリを自分で操作して確認する代わりに、エージェントにページを開き、移動し、フィルターを適用して、その結果を読み上げてもらえます。何を確認したかの要約も受け取れます。会話を離れずに機能の動作を確かめる最も早い方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。つまり、エージェントはボタン、リンク、リスト項目など、ラベル付きの構造化された要素を基に推論します。支援技術と同じ情報を使うため、簡単な機能確認がアクセシビリティの基本チェックにもなります。

サーバーを接続し、アプリを起動した状態で、Copilot に次の手順でフィルタリング機能を確認するよう依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを介してブラウザーを起動し、各手順を実行して結果を報告します。報告内容を課題の受け入れ条件と照らし合わせます。動作に問題がある場合は、プルリクエストを作成する前に、Copilot に追加質問をしたり、コードを修正するよう依頼したりしてください。

> [!NOTE]
> このテストには、`http://localhost:4321` でアプリを起動しておく必要があります。開発サーバーを停止している場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーのダウンロードが必要になる場合があります。ブラウザーがないと報告された場合は、`npx playwright install chromium` を実行して再試行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームのレビュー用にプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace で **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを選択してコミット メッセージを生成します。

   ![変更内容が表示された Source Control パネルのスクリーンショット。](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

プルリクエストの作成方法はいくつかあります。github.com や GitHub コマンド ライン インターフェイス (CLI) を使う方法などです。GitHub Copilot を使っているので、PR の作成も依頼しましょう。Copilot は関連する課題を探し、その課題に関連付けて PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub のツールを使えるよう、エージェント ドロップダウンで **Agent** が選択されていることを確認します。
3. 次のように Copilot に PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報を収集して操作を実行できるようにします。GitHub のツールを初めて使用するときは、GitHub への認証を求められることがあります。画面の指示に従ってください。
5. Copilot が課題を検索し、適切な課題を見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択してプルリクエストを確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

お疲れさまでした。この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot を拡張する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- Playwright MCP サーバーを使って、リリース前にブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成しました。

MCP サーバーを使って機能をテストしましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は[カスタム エージェント][next-lesson]を作成し、アクセシビリティ レビューなどの対象を絞ったタスクを効率化しましょう。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
