---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウド上で非同期に作業できます。GitHub 上で作業を割り当てると、cloud agent がバックグラウンドでリポジトリを調べて変更を加え、プルリクエストを作成します。その間、別の作業を続けられます。

各演習では、まず cloud agent が従うカスタム指示を追加し、次に GitHub issue を割り当てて実装を依頼します。カスタム エージェントを確認してアプローチを調整し、エージェント ダッシュボードからセッションの進行を監視および指示します。最後に、プルリクエストをレビューして作業を反復します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム指示][ex1] | コンテキスト | cloud agent が従うカスタム指示を追加する |
| [2. Cloud Agent][ex2] | 非同期エージェント | Copilot cloud agent に issue を割り当てる |
| [3. カスタム エージェント][ex3] | 専門エージェント | カスタム エージェントを確認して使用する |
| [4. エージェントの管理][ex4] | 監視 | エージェント セッションを監視して指示する |
| [5. 反復][ex5] | レビュー | PR をレビューし、Copilot の作業を反復して次の手順を選ぶ |

## 前提条件

このワークショップに参加する前に、次の準備ができていることを確認してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランのいずれかがある GitHub アカウント
- [ ] アカウントまたは組織で Copilot cloud agent が有効になっていること

> [!TIP]
> 有料プランを利用していない場合、学生として認証された方は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、どのワークショップ構成でも最後まで完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free は対象外です。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> このワークショップでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ワークショップ](../cli/)または [VS Code ワークショップ](../vscode/)を参照してください。

## はじめに

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
