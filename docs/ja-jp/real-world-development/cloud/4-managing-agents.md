---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の数回の演習では、Copilot cloud agent にユーザーエクスペリエンスの改善や機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。その 1 つが、GitHub.com の[エージェントページ][agents-page]です。このミッションコントロールでは、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を出したりできます。

このレッスンでは、次の内容を学びます。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに指示を出して、機能を追加するよう依頼します。

## シナリオ

ハイコントラストモードの作成をエージェントに割り当てた後、チームはライトモードも追加するよい機会だと気付きました。サイトのスタイルを更新し、切り替え機能を追加する作業をすでに進めているため、ライトモードも含めるのが自然です。エージェントの作業を誘導し、ハイコントラストモードだけでなくライトモードも追加するよう依頼します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot エージェントページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方に表示されるタスク一覧を確認します。Copilot に割り当てたタスクとして、次のようなものが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲームを変更するための API を生成する。
   - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを 1 つ選択し、Copilot が実行した作業を確認します。次のような内容があります。
   - リポジトリからコードをチェックアウトする。
   - Copilot の作業環境を作成する。
   - MCP サーバーを設定する。
   - 割り当てられたタスクを完了するため、さまざまな手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR や変更されたファイルを確認し、作業をさらに監視できます。

## Cloud agent に指示を出す

実行中のタスクを確認できたので、Copilot がハイコントラストモードに取り組んでいる間に、ライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードの追加に関連するセッションを選択します。Copilot が付けた名前や作業状況によって、タイトルは異なります。

   ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことが表示されるまで、数分間セッションを確認します。次のようなメッセージが表示されたら、作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![エージェントページで、作業中の cloud agent に指示するダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業計画に追加することを確認します。

## Copilot に作業を任せる

前回と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を終えてから、新しい依頼を計画に取り込みます。

今回も完了まで数分かかります。その間に、ここまで学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視し、誘導するための中心的な場所である Copilot エージェントページを確認しました。このミッションコントロールでは、進行中のタスクや完了したタスクを追跡し、作業内容を確認できます。進行中のタスクの範囲を変更したり、追加のガイダンスを伝えたりすることもできます。

次の内容を学びました。

- ミッションコントロールとエージェントページで cloud agent のタスクを監視する。
- 進行中のセッションに指示を出して機能を追加する。

Copilot がアクセシビリティ機能の作業を完了したら、Copilot が作成したプルリクエストを[反復して改善する][next-lesson]方法を確認しましょう。

## リソース

- [Copilot エージェントページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
