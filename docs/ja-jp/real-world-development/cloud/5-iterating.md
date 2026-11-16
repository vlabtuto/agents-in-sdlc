---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスの改善や機能追加を目的に、GitHub Copilot にいくつかのタスクを依頼しました。コードへのドキュメント追加、デザインチームが試せる関連ゲーム機能の作成、ハイコントラストモードとライトモードを含むアクセシビリティ機能の実装を依頼しました。コードの変更内容を確認し、必要に応じて Copilot にフィードバックを伝え、作業を改善してもらいましょう。

### シナリオ

何度も説明してきたように、生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全員にレビューを依頼する前に、GitHub Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の提案を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は監督なしで非同期にタスクを実行するため、安全性を確保するためのセキュリティ制約が設けられています。たとえば、次のような制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスはコード用のブランチに限られます。
- Cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは、デフォルトで制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーへのアクセスも制限の対象です。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が最初に作成したプルリクエスト (PR) を確認します。コードにドキュメントを追加する PR です。GitHub.com の標準的な PR インターフェイスを使って確認します。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot はデフォルトで、外部リソースや外部 MCP サーバーへのアクセスが制限されているため、これは**想定どおりの動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら、**Files changed** タブを選択して変更内容を確認します。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc ドキュメントコメントやその他のドキュメントを含む、更新されたコードを確認します。変更内容は状況によって異なります。

   変更を確認するときは、TSDoc ドキュメントコメント、TypeScript の規約、コメントヘッダーに注目してください。これらは演習 2 の最初に確認したカスタム指示ファイルによるものです。

6. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. プルリクエストのチェック欄にワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するのは一方通行ではありません。チームメンバーと同じように、プルリクエストのコメントやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、変更に対応するための新しいセッションを開始します。結果は確定的ではないため、必ずこう依頼するという文面はありません。次のような変更を Copilot に依頼できます。

- 各コードファイルの先頭に、ファイルの役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc ドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. **@copilot** をメンションして、生成されたドキュメントの変更をコメントで依頼します。上記の例を使うか、コードベースのドキュメントに関して別の変更を依頼します。
2. **View Session** を選択して、Copilot が作業する様子を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して、変更内容を確認します。

納得できるまで、Copilot と繰り返し作業できます。満足できたら、PR をドラフトからレビュー可能な状態に変更して、`main` ブランチにマージします。

![PR をレビュー可能な状態に変更](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue について、Copilot が作成した PR を確認しましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
6. ワークフローが承認待ちであることを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. プルリクエストのチェック欄にワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **任意:** このブランチを Codespace に切り替えて、関連ゲーム機能を手動でテストすることもできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます。例: **copilot/fix-8**)。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に変更を依頼して、繰り返し改善できます。たとえば、表示する関連ゲームの数を調整するよう依頼したり、コメントヘッダーと TSDoc ドキュメントコメントを追加するよう依頼したりできます (このタスクはカスタム指示を更新する**前**に割り当てたことを思い出してください)。**Conversation** タブにコメントを追加すると、Copilot がその内容を確認し、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントを使って実装した機能を確認します。この PR には演習 3 で依頼したハイコントラストモードと、演習 4 でミッションコントロールから追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 実装をレビューするときは、特に次の点を確認します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保存するためのローカルストレージの使用。
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. プルリクエストのチェック欄にワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **任意:** このブランチを Codespace に切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードの切り替えを試します。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに沿ってこれらの機能を実装するよう Copilot を導いたことがわかります。アクセシビリティ上の懸念や改善点がある場合は、これまでと同じようにコメントで **@copilot** に変更を依頼できます。

## 任意の演習 — さらにタスクを委任する

Cloud agent は、バックログにある実際のタスクを任せ、その間に別の作業に取り組むときに力を発揮します。練習のため、リポジトリにいくつかの Issue を登録し、Copilot に割り当ててみましょう。たとえば、次のようなタスクがあります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のハーネスを完了しました。このレッスンでは、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、cloud agent の出力に後で反映される内容を学びました。
- **Copilot cloud agent に Issue を割り当て**、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタムエージェントを**作成して使用し**、ハイコントラストモードとライトモードの切り替え機能を追加しました。
- **Copilot エージェントページをミッションコントロールとして使用し**、実行中のアクセシビリティセッションを監視して誘導しました。
- **cloud agent のプルリクエストをレビューして反復的に改善し**、`@copilot` に変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent のハーネスを完了しました。さらに学びたい場合は、ほかのハーネスで今回の内容を補完できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から直接 Copilot Chat のエージェントモードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェントスキル、カスタム QA エージェント、Playwright MCP、Agent Merge、オプションの `/delegate` による cloud agent への連携を使って、ターミナルからレビュー済みの変更を 2 つ実装します。

自分のリポジトリでは、次のようなことを試してください。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる。
- 別の分野に特化したカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
