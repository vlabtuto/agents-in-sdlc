---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

最も簡単なアプリケーションの更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は通常、必要なファイルを探し、変更を加え、テストを実行し、デバッグし、変更漏れのファイルを特定してさらに更新する、という作業を繰り返します。作業は次々と続きます。

そこで役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、IDE 内でより自律的に作業できるように設計されています。開発者と同じように、まず既存のプロジェクト構造を調べ、必要な更新を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、Agent Mode と cloud agent は異なる用途向けに作られています。Agent Mode は IDE 内で動作するため、すばやいフィードバックと対話が可能です。Cloud agent はチームの一員のように非同期で作業するピア プログラマーとして設計されており、課題やプルリクエストを通じてやり取りします。

この演習では、次の方法を学びます。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数ファイルにまたがる変更を行います。
- GitHub Copilot Agent Mode で UI とデータ レイヤーに新機能を実装します。
- Copilot Agent Mode が生成した変更とテストを、コードベースにマージする前に確認します。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリや出版社で絞り込めるようにします。前の演習では出版社ヘルパーを追加しました。今回は Copilot Agent Mode を使って、データ レイヤー、UI、テストの残りの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを調べ、現在の機能を確認しましょう。

この Web サイトは、ボード ゲーム向けの開発者テーマのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示できます。Web サイトは単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。ページはフロントマターから Drizzle ORM を介してローカル SQLite データベースを直接参照します。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、インタラクティブな機能は標準 DOM API を使う、小規模で対象を限定した Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. 前の演習の後に戻った `main` ブランチの Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナル ウィンドウを開きます。
3. フィルタリング作業用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、Astro サーバーのアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは Web サーバーを実行中の Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認しましょう。主な機能は次のとおりです。

- **ホーム ページ**: ボード ゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、そのタイトル、説明、出版社、カテゴリなどの情報を表示する詳細ページが開きます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

Web サイトはすでに動作しますが、新しい機能を追加して改善しましょう。まずバックログを確認します。テンプレートからプロジェクトを設定すると、課題のバックログが自動的に作成されています。GitHub Copilot に項目を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** エージェントは、自律的に計画を立て、複数ファイルにわたる変更を実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨しているわけではありません。ワークショップの作成には Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案されるコードにある程度一貫性を持たせ、演習を進めやすくすることを意図しています。ただし、LLM の出力は確率的なため、ワークショップの例とは異なる提案が表示されることがあります。これは通常の動作であり、想定内です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使用する場合がありますが、タスク自体は完了できるはずです。

3. Copilot に次のプロンプトを送信し、課題のバックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての課題を一覧表示するコマンドを実行します。
5. 生成された課題の一覧を確認します。

Copilot がユーザーに最も役立つと考える順に項目を優先付けしていることを確認してください。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilot に作業のコンテキストを伝える指示ファイルを確認しましょう。Web サイトに機能を追加する方法が記載された [user interface (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加に関する全体的な指針を確認します。次のような内容があります。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティに関する原則。
   - Astro、Tailwind CSS、Drizzle などのファイル タイプに関する指示ファイルへのリンク。

> [!TIP]
> 指示ファイルからは、別の指示ファイルとプロジェクト内のファイルの両方を参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用しやすい小さなファイルに分割したり、参考になる例を指定したりできます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の 3 つの作業が必要です。

- データ レイヤー (`src/lib/`) にフィルタリング ロジックを追加または調整します。
- フィルタリングの動作をテストするテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

さらに、コードベースにマージする前にテストを実行し、すべて成功することを確認する必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. 現在の Copilot の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット。](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルとして引き続き **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログ内の関連課題に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がまずプロジェクトを調べ、機能に関連するファイルを探す様子を確認します。データ レイヤーのヘルパー、UI、テストが見つかり、続いてファイルの変更とテストが始まります。

   ![Copilot がプロジェクト ファイルを調べている様子を示すスクリーンショット。](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行など、複数の作業を行います。タスクやコードベースの複雑さによっては、数分かかることがあります。作業中、コード エディターに **Keep** と **Undo** が表示されることがあります。完了するとすべての変更に **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot に促されたら **Continue** を選択し、テストを実行します。

   ![Copilot Chat ペインでテストの実行を確認するダイアログを示すスクリーンショット。](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はコード生成とテストを繰り返し、タスクが完了してエラーが検出されなくなるまで作業します。

   ![Copilot Agent Mode とのチャット セッションの完了状態を示すスクリーンショット。](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ず確認することが重要です。

10. Web サイトを起動しているブラウザーに戻り、新しい機能を確認します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

お疲れさまでした。この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot Agent Mode で UI とデータ レイヤーに新機能を実装できます。
- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数ファイルにわたる変更を行えます。
- Copilot Agent Mode が生成した変更とテストを、コードベースにマージする前に確認できます。

次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### 追加の演習 — ページ分割を実装する

ゲームの一覧が増えると、ページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割を実装するよう依頼してください。プロンプトを作成するときは、次の点を考慮します。

- 既存の指示ファイルを使うなど、既存のベスト プラクティスに従います。
- ユーザーがページ サイズを選べるようにするか、値を固定するかなど、ページ分割の方法を検討します。
- 希望する実装に必要な指針を Copilot に伝えるプロンプトを作成します。
- GitHub Copilot に変更を依頼し、コンテキストを提供しながら何度か繰り返し作業する必要があるかもしれません。Copilot と作業する際には通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code の Agent Mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
