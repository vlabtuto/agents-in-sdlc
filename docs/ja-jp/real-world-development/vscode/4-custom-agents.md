---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成してエージェントを定義すると、特定の作業をより効果的に実行できるよう、対象を絞った指示、ベスト プラクティス、コーディング パターン、専門知識を Copilot に与えられます。チームの専門知識を再利用可能なエージェントにまとめることもできます。たとえば、[WCAG][wcag] への準拠を確認するアクセシビリティ エージェント、安全なコーディング手法に従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントなどです。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` フォルダーにある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、役割が一部重なる部分があります。どちらも主に Markdown ファイルで定義され、AI に作業方法を指示します。最も分かりやすい区別は、**カスタム エージェントは作業者、スキルはツール**ということです。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業の一環としてスキル (ほかのエージェントを含む) を呼び出すよう調整できます。この演習では、アクセシビリティ カスタム エージェントがサイトをレビューし、アクセシビリティ ガイドラインに沿って更新します。その作業の一環として、プルリクエストのワークフロー スキルや、テストを実行・管理するスキルなどを呼び出せます。

> [!NOTE]
> カスタム エージェントの作成方法は 1 つではありません。AI のほかの使い方と同じように、環境やシナリオに合わせて試しながら調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/
## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにすることに取り組んでいます。最近、現在のダーク テーマは文字と背景のコントラストが不十分で、読みにくいというフィードバックが寄せられました。このアクセシビリティの問題に対処するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。カスタム エージェントを使って機能を生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ向けのカスタム エージェントがあらかじめ用意されています。内容を確認し、Copilot への指針を把握しましょう。

Codespace に戻り、ターミナルを開いて、アクセシビリティ作業用の新しいブランチを `main` から作成します (フィルタリングの PR とは分けて作業します)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` を含むフロントマターは、カスタム エージェントに必須です。

3. 続くセクションを確認します。次のような内容があります。
   - アクセシブルな Web サイトのコードを生成する際の主な役割。
   - アクセシビリティに関するベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタム エージェントを使う

VS Code では、`.github/agents` にあるすべてのカスタム エージェントが Copilot Chat ビュー下部のエージェント ドロップダウンに表示されます。カスタム エージェントを選択すると、そのエージェントの指示とツールに基づいたチャット セッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェント ドロップダウンで、まだ選択されていない場合は **Agent** を選択します。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. チャット ビュー下部のエージェント ドロップダウンを選択します。アクティブなエージェント (既定では **default**) が表示されています。
3. 使用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティ エージェントに次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat が作業を開始します。コードベースを調査して変更を提案し、プロジェクト ファイルを編集します。編集内容は、ファイル パスと差分とともにチャット内に表示されます。
6. 編集が反映されるたびに、エージェントが変更した作業ファイル一式を示す **Files changed** の表示が更新されます。

> [!NOTE]
> この作業には数分かかることがあります。Copilot は実際にリポジトリを変更します。作業中、Astro コンポーネント、CSS、関連するテストなど既存のファイルが編集されます。

次の演習では、作業中の変更を確認し、方向を調整します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野向けの AI アシスタントである [GitHub Copilot のカスタム エージェント][custom-agents]について学びました。カスタム エージェントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、Copilot がさまざまな作業をより効果的に実行できるように指示できます。

次のことを確認しました。

- カスタム エージェントの定義方法。
- Copilot Chat の Agent Mode でカスタム エージェントを使う方法。

次は[エージェントの作業を監視して調整][next-lesson]します。変更内容を確認し、同じセッションにライト モードの切り替えを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントを監視して管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
