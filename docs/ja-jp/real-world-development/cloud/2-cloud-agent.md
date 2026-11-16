---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなど、さまざまな例があります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行します。作業が完了すると、人間の開発者が確認できるドラフトのプルリクエスト (PR) を作成します。これにより、手間のかかるタスクを任せて開発プロセスを加速し、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次の内容を学びます。

- コード生成の環境をカスタマイズする方法。
- 操作を安全に行う方法。
- Issue の範囲を明確に定めることの重要性。
- Copilot に Issue を割り当てる方法。

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。最初のサイトを開発した委託先は、ドキュメントを十分に整備していませんでした。実際、ドキュメントはほとんどありません。まず、アプリケーションのすべてのエクスポートされた関数に TSDoc ドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのゲームを表示して、ユーザーがゲームを見続けられるようにします。洗練された実装はまだ必要ありません。UX の受け入れテストに使えるものを求めています。このタスクは現在ブロッカーですが、ほかに優先度の高い Issue があります。

どちらも、優先度を下げられがちなタスクの例です。Copilot cloud agent に割り当てると、非同期で作業を進められます。開発者は別のタスクに集中し、あとで Copilot の作業を確認して、期待どおりの内容かどうかをチェックできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。人間の開発者と作業する場合と同様に、[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法などがあります。割り当てると、Copilot は進捗を追跡するドラフトのプルリクエストを作成し、環境をセットアップしてタスクに取りかかります。実行中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 適切な範囲の指示が重要な理由

GitHub Copilot に魔法はありません。考える手間を省けるわけではありません。一見簡単な操作でも、詳しく見ると複雑さが明らかになります。そのため、[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、状況に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と、それが Copilot を導く仕組みを確認しました。カスタム指示の演習をすでに行った場合は、`.github/copilot-instructions.md` が機能することを確認しています。まだの場合は、ここで少し確認しておきましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認しておくと、あとでその効果を見つけやすくなります。

GitHub の Web UI でリポジトリを開くか、Codespace を起動済みの場合はそこで、次のファイルを確認します。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターにより、これらの指示が `**/*.test.ts` ファイルに適用されることを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、TSDoc ドキュメントコメント、TypeScript の規約、コメントヘッダーが反映されているかを確認します。これらのルールは指示ファイルに記載されています。後の演習で PR をレビューするときにも、改めて確認します。

## Copilot cloud agent の開発環境を設定する

誰がコードを書く場合でも、通常は特定の環境が必要です。作業を始める前に、状態を整えるセットアップスクリプトを実行することもあります。Copilot にタスクを割り当てる場合も同様です。Copilot はソフトウェアエンジニアのように作業します。

Cloud agent は、作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` ファイルで設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に実行される環境をカスタマイズできます。これにより、必要な開発ツールや依存関係を利用できるようになります。このラボの流れを簡単にし、学習機会を提供するために、事前に設定されています。単一の Astro アプリで Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシードを適用した SQLite データベースを利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 必要な手順に必要な最小限の権限を設定します。Copilot は操作用に独自のトークンを受け取ります。
    permissions:
      # たとえば依存関係をインストールするため、セットアップ手順の中でリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップ手順でリポジトリをクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ — Node.js (アプリ全体は Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを利用できるように、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローファイルと同じ形式ですが、次の点に注目してください。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーにより、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot が実行するまで待たずに、ワークフローが正常に動くかテストできます。

## ドキュメントを追加する

誰もがドキュメントの重要性を理解していますが、多くのプロジェクトでは情報が古かったり、ドキュメントがまったくなかったりします。このような技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成を得意とするため、Copilot cloud agent に割り当てるのに適した Issue です。バックグラウンドで必要なドキュメントを作成してくれます。後の演習で、その作業を確認します。

1. 新しいブラウザータブで、github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく生成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような内容です。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が To Do リストを作成したことを確認できます。

> [!NOTE]
> Copilot が作成する To Do リストが PR に表示されるまで、数分かかることがあります。Copilot は環境を作成し (前述のワークフローを実行します)、プロジェクトを分析して、タスクに取り組む方法を決めています。

13. リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が出るまで、ウィンドウの再読み込みが必要な場合があります。

16. ライブセッションをスクロールして、Copilot がどのように問題を解決しているかを確認します。コードを調べて状態を理解する様子、計画を立てる前に思考して方針を決める様子、コードを作成する様子などを確認できます。

この作業には数分かかるでしょう。Copilot cloud agent の主な目的の 1 つは、タスクを非同期で実行し、私たちがほかの作業に集中できるようにすることです。この機能を活用するため、Copilot cloud agent にもう 1 つのタスクを割り当ててから、アプリケーションの機能を追加するコードを書きます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

前述のとおり、GitHub Copilot cloud agent の大きな利点の 1 つは、作業を分担できることです。デザインチーム向けに関連ゲームのセクションを追加するのに時間がかからないとしても、その時間をほかのタスクに使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次のテキストを入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示されます。Copilot が作業中であることを示しています。

![目の絵文字で Copilot が Issue に取り組んでいることを示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェアエンジニアのように作業するため、常に監視する必要はありません。完了後に確認すれば十分です。ここからは、カスタムエージェントの作成と使用を見ていきましょう。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に Issue を割り当てると、非同期で作業できます。技術的負債の解消、新しい機能の作成、別のフレームワークへのコード移行などに Copilot を活用できます。

次の内容を学びました。

- コード生成環境のカスタマイズ。
- 操作を安全に行う方法。
- Issue の範囲を明確に定めることの重要性。
- Copilot への Issue の割り当て。

Copilot cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
