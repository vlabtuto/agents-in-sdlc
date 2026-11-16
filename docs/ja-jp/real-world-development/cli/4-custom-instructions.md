---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを進める必要がある場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]では、必要なコードだけでなく、コードの構成方法も指定できます。フィルタリングを構築したので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示と、パスを対象とする指示ファイルがエージェントに適用される方法を確認します。
- コーディング標準を守るために指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発の進め方に関するガイドラインと要件があります。たとえば、次のようなものです。

- コメントでは、コードの言い換えではなく、意図と自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。該当する場合は、注入可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントでは、`Props` 契約を説明します。関連コードが変更されたときはコメントも最新の状態にします。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの開発慣行に沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示を使うと、コンテキストや好みを Copilot に伝え、コーディング スタイルや要件を理解しやすくできます。たとえば、コーディング規約、ライブラリ、コメントの書き方を指定できます。リポジトリ全体に適用する指示も、特定の種類のファイルだけに適用する指示も作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の指示ファイルです。多くのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイドラインを記載します。

> [!NOTE]
> その他の指示形式やサポート状況は、環境によって異なります。特定の形式を前提にする前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すでに指示ファイルが含まれています。変更を加える前に内容を確認し、変更後の影響と比較しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで、ターミナルではなく `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この指示は、このリポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開いてファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。ここでは、指示が適用されるファイルを決める glob が設定されています。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存のプロジェクト ファイルへのリンクを確認します。これにより、大きな指示セットを小さな再利用可能ファイルに分け、参考例を Copilot に示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、追加のガイダンスが必要です。コアの `copilot-instructions.md` を変更し、新しく生成される TypeScript に TSDoc コメントを追加するようにします。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存したら、フィルタリングの会話を終了して再開し、機能のコンテキストを保ったまま新しいガイダンスを読み込みます。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリ ルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示された場合は、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードの説明として正確であることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを得る方法を確認し、機能に新しい標準を適用しました。具体的には、次のことを行いました。

- リポジトリの指示と、パスを対象とする指示ファイルがエージェントに適用される方法を確認しました。
- コーディング標準を守るために指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
