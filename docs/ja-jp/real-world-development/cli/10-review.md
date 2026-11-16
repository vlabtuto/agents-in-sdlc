---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用できるカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローに取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを調べ、用意されていたフィルタリングの issue を見つけました。
- 星評価を追加し、転送ブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの issue から始め、Plan モードでアプローチを決め、Autopilot モードで構築し、Interactive モードで結果をレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 要件、テスト カバレッジ、スキルの結果、ブラウザーの証拠を評価する QA カスタム エージェントを作成して選択しました。
- フィルタリングの変更全体をレビューし、フィルタリング PR の Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意の cloud agent への委任に使うスラッシュ コマンドを確認しました。

## リリースしたもの

このワークショップには、それぞれ更新済み `main` から作成したブランチで進める、2 つの PR の節目があります。

1. **星評価:** 既存の `starRating` と、評価がない場合の状態をゲーム カードに表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズして QA プロファイルを作成し、関連するテストを追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔に進めるため、この作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA エージェントは PR の前に、これらの結果を要件とテスト カバレッジのレビューにまとめました。

追加されたテストが本当に不足していた箇所を補うべきですが、QA の実行で新しいテストが必要ない場合もあります。マージを許可する前に、コードと検証結果を確認し、変更後は影響を受ける検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整してください。指示は標準を定め、スキルは繰り返し行うタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った **モードとモデル** を選びます。構築前にアプローチを検討するには **Plan**、範囲を絞った変更に対話しながら取り組むには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように作るかを明確に説明すると、出力が大きく変わります。

## さらに学ぶ

基本的なワークフローを学びました。Copilot CLI の追加機能として、次のコマンドも確認してみてください。

- `/review` でコードレビュー エージェントに変更の分析を依頼します。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールの使い方を上達させる最善の方法は、使い続けることです。本番コードでも、趣味のコードでも、何年も作りたいと思いながら手を付けられていない小さなアプリでも使ってみましょう。学んだことをチームと共有し、チームのメンバーからも学びましょう。そして、ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
