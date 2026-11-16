---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app を使って Pages ワークフローを設定し、プルリクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages で Space Quiz を公開しましょう。Copilot はワークフローの準備とデプロイを支援しますが、変更をレビューし、サイトが公開されたことを確認するのはあなたの役割です。

このレッスンでは、次のことを行います。

- Copilot に GitHub Pages のデプロイを設定してもらいます。
- マージする前に、ワークフローとプルリクエストをレビューします。
- デプロイの状況を確認し、公開されたサイトを検証します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、まだ誰でも簡単に試せる状態ではありません。GitHub Pages で単一ファイルのサイトを公開すれば、サーバーやランタイム依存関係を追加せずに、公開 URL から利用できるようになります。

## Copilot にデプロイの準備を依頼する

> [!WARNING]
> プルリクエストの作成時に、コードがコミットおよびプッシュされることがあります。プロンプトを送る前に、承認設定と送信先のリポジトリを確認してください。**Approve all** を選択している場合、個別の確認が表示されないことがあります。

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## ワークフローとプルリクエストをレビューする

1. リポジトリに README がない場合に限り、README が追加されていることを確認します。
2. ワークフローのビルドとデプロイの手順、トリガー、権限を含め、プルリクエストの変更をレビューします。
3. ワークフローが未マージのプルリクエストではなく、デフォルト ブランチからデプロイすることを確認します。
4. 必要な修正を依頼し、更新された差分をレビューします。マージを許可する前に、必須のプルリクエスト チェックが成功するのを待ちます。

## マージとデプロイを許可する

> [!WARNING]
> 次のプロンプトを送信すると、レビュー済みプルリクエストのマージ、リポジトリの Pages 設定の変更、サイトの公開をエージェントに許可します。レビューを完了し、リポジトリとプルリクエストを確認してから送信してください。承認設定によっては、これらの操作に別途確認が求められないことがあります。

`<PR_NUMBER>` をレビューしたプルリクエストの番号に置き換えてから、別のプロンプトとして送信します。

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![3 段階の図: マージ済みの space-quiz プルリクエスト、Build website・Upload artifact・Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website 実行、yourname.github.io/space-quiz/ で公開された Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを確認する

1. GitHub Actions のデプロイ実行が成功するまで確認します。
2. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages を有効化できない場合やプルリクエストをマージできない場合は、エージェントが示す手順に従います。デプロイが成功し、公開サイトが読み込まれるまでは、作業を完了とみなさないでください。

## まとめと次のステップ

Space Quiz が公開され、エージェントの報告に頼らずデプロイを確認できました。次は[レッスン 13: レビューと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
