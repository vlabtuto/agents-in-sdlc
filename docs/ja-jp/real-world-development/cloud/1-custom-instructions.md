---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際はコンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにしておきます。[指示ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」実行するかも伝えられます。

この演習では、次の内容を学びます。

- リポジトリのカスタム指示ファイルとパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここではプロンプトの実行前と実行後を比較しません。Copilot cloud agent は GitHub の Issue に対して非同期で動作するため、リアルタイムで並べて効果を示すのが難しいためです。このハーネスの後半で cloud agent が作成するプルリクエストを確認すると、指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。たとえば、次のようなものです。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードにし、現代的な印象にします。
- コードには TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発方針に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、Copilot がコーディングスタイルや要件を理解しやすくなるよう、コンテキストや好みを伝えられます。コーディング規約やライブラリ、含めたいコメントの種類などを指定できるため、より適切な提案やコードスニペットを得るのに役立ちます。リポジトリ全体に適用する指示も、特定のファイル種別に適用する指示も作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送信される単一の指示ファイルです。Copilot に送るチャットや CLI のリクエストの多くに関係する、プロジェクトレベルの情報を記載します。技術スタック、開発対象の概要、ベストプラクティス、全般的なガイダンスなどが該当します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイル種別に対するガイドラインを記載します。TypeScript や Astro などの言語、UI コンポーネントやユニットテストの作成などのタスクを対象にできます。

> [!NOTE]
> IDE で作業する場合、指示ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示ファイルと、`applyTo` フロントマターが設定された `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[対応する指示ファイル][custom-instructions-support]をご覧ください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法を詳しく説明することはこのレッスンの範囲外ですが、サンプルプロジェクトには代表的な構成例があります。基本的には、次のようにします。

- `copilot-instructions.md` には、開発対象の説明、プロジェクト構成、全般的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種別や、特定のタスクに関する指示を記載します。
- 自然な言葉を使い、明確なガイダンスにします。コードの良い例と避けるべき例を示します。

AI の使い方に唯一の正解がないのと同じように、指示ファイルにも決まった形式はありません。試しながら、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使う各プロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトの指示ファイルを見ると、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを作成してもらうこともできます。各環境で手順は異なります。たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。各環境のレッスンで、関連する手順を説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめたリポジトリ [awesome-copilot][awesome-copilot] をご覧ください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** には、入れ子になった **GitHub Actions Workflows** のガイダンスがあります。これらの内容は Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドで、指示を適用するファイルを決める glob が設定されています。ここでは、`**/*.test.ts` に一致するファイルなど、TypeScript のテストファイルが対象になります。
5. このプロジェクトでユニットテストを作成するための指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの別の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示セットを小さく再利用可能なファイルに分け、Copilot がコードを生成するときに参考にできる例を示せます。ここにあるパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc ドキュメントコメントとファイルコメントヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は 1 つです。コード内のドキュメントを TSDoc ドキュメントコメントとファイルレベルのコメントヘッダーとして記載する、プロジェクト全体のルールを追加します。このハーネスの後半で Issue を割り当てると、cloud agent はこのルールを読み取ります。

編集の前に、作業用のブランチを作成します。cloud agent は Issue の対象ブランチにある指示を読み取りますが、ここでは変更をきちんとコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前に説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を伝えるためのファイルです。コードの提案を改善するために、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、インラインドキュメントのルールはありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加します。これにより、Copilot にファイルコメントヘッダーと TSDoc ドキュメントコメントを追加するよう指示できます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に作成して全般的なガイダンスを記載することも、言語、ファイル種別、タスクに合わせた `*.instructions.md` ファイルとして作成することもできます。ここで追加したドキュメントコメントのルールのような、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## 指示をコミット、プッシュ、マージする

Cloud agent は、Issue が対象とするブランチの指示ファイルを読み取ります。次の演習で Copilot に Issue を割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が指示を参照できるように、変更を `main` に取り込む必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で割り当てた Issue に取り組むとき、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを作成せずに `main` にコミットしてもかまいません。ブランチを使う手順は、実際のプロジェクトでの作業方法を再現しています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、リポジトリ全体に適用する新しい標準を `.github/copilot-instructions.md` に追加しました。このハーネスの後半で cloud agent が作成するプルリクエストを確認すると、その標準が適用されていることがわかります。

次は、[Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
