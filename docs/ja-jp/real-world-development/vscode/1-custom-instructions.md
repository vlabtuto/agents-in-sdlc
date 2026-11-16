---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにしておきましょう。Copilot に特定のコンテキストを共有する方法はいくつかあります。その中でも重要なのが[指示ファイル][instruction-files]です。指示ファイルを使うと、コード生成に関する指針を伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示ファイルとパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示を適用した状態で、フィルタリング用の最初のデータ スライス (publishers ヘルパー) を生成します。
- リポジトリ全体に適用する新しい標準を `.github/copilot-instructions.md` に追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認します。
- 指示の更新とフィルタリング用スライスを `main` にコミットし、次の演習で Copilot が更新済みの指針を使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準の一部と異なる場合があります。Copilot の出力は決定的ではありません。この演習では、指示の更新後に動作がどのように変化するかを確認することが目的であり、出力を一字一句一致させることではありません。

## 指示ファイル

### シナリオ

優れた開発チームである Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データ レイヤーには必ずユニット テストを用意します。
- UI はダーク モードにし、現代的な印象にします。
- TSDoc のドキュメント コメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発手法に沿ったタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、Copilot がコーディング スタイルや要件を理解できるように、コンテキストや設定を指定できます。コーディング規約、ライブラリ、コードに含めたいコメントの種類などを伝えることで、より適切な提案やコード スニペットを得られます。リポジトリ全体または特定のファイル タイプに指示を設定できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。Copilot に送るチャットや CLI のリクエストの大半に関係する、プロジェクト レベルの情報を記載します。たとえば、技術スタック、開発対象の概要、ベスト プラクティス、全体に適用する指針などです。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイル タイプに利用できます。TypeScript などの言語や、UI コンポーネントやユニット テストの作成などのタスクに関する指針を指定できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示ファイルと `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は、AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[対応する指示ファイルの形式][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成に関する詳しい説明はこの演習の範囲外ですが、サンプル プロジェクトには代表的な方法が示されています。基本的には、次の点を意識してください。

- `copilot-instructions.md` には、構築するものの説明、プロジェクトの構造、全体に適用するコーディング標準など、プロジェクト レベルの指針を記載します。
- `*.instructions.md` ファイルには、ユニット テスト、Astro コンポーネント、データ レイヤーなどのファイル タイプや、特定のタスクに関する指示を記載します。
- 自然な言葉で、明確な指針と例を示します。コードの良い例と避けるべき例を含めてください。

指示ファイルの作成方法は 1 つではありません。AI の使い方と同じように、試しながらプロジェクトに合う方法を見つけましょう。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトには[UI の更新][ui-instructions]や[Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを生成させることもできます。各環境で方法は異なります (例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。この演習では、該当する場合に利用方法を説明します。
>
> テンプレートやひな形を探していますか。[awesome-copilot][awesome-copilot]では、カスタム エージェントや指示ファイルなど、さまざまなリソースを確認できます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認しましょう。基本となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** の指針もあります。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob パターン (リポジトリ ルートからの相対パス) を指定しています。たとえば、`**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトのユニット テスト作成に関する指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。記載されたパスはリポジトリ ルートではなく、指示ファイルからの相対パスです。大きな指示セットを小さく再利用可能なファイルに分け、コード生成時に参考にする例を示せます。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc のドキュメント コメントとファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ここではベスト プラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを適用した状態でプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習で開いたエディター タブをすべて閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に知らせます。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。多くの場合、ヘルパー ファイルとテストが変更されます。
6. 提案されたヘルパーは、最初の引数として `db` クライアントを受け取り、出版社の型付き配列を返す関数です。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に基づいています。
7. 提案されたコードに TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の出力は確率的なため、指示がなくてもドキュメント コメントが追加される場合があります。その場合も問題ありません。ここで重要なのは、指示を更新した後に一貫性が高まることです。

## リポジトリの新しい標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コードの提案を改善するために、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. **Code formatting requirements** セクションを見つけます。27 行目付近にあります。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールがありません。そのため、先ほどのヘルパーにはドキュメント コメントが追加されませんでした。
3. 既存の標準の直下に、次の Markdown を追加し、ファイル コメント ヘッダーと TSDoc ドキュメント コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ レベル (`.github/copilot-instructions.md`) に置いて全体に適用することも、特定の言語、ファイル タイプ、タスク向けの `*.instructions.md` ファイルとして作成することもできます。今回追加したドキュメント コメントのルールのように、プロジェクト全体の標準はリポジトリ レベルのファイルに記載します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をリセットします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 今度は、提案されたファイルの先頭に次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot を誘導し、次の演習で利用する実際のコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもほかのアセットと同様に、通常のソース管理で管理します。指示ファイルとブランチをリポジトリにコミットしてプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナル ウィンドウを開きます。
2. ターミナルで `git status` を実行し、指示の更新とヘルパーの変更が含まれていることを確認します。

   ```bash
   git status
   ```

3. ターミナルで、指示の更新とヘルパーの基盤に関して Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加してください。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の課題に関連付けてプルリクエストを作成します。手動で行う代わりに、Copilot に課題を探させ、課題をクローズするプルリクエストを作成し、マージするように依頼します。

> [!NOTE]
> この演習では、Copilot が GitHub 上で操作するのは初めてです。プロジェクト テンプレートでは GitHub のツールがあらかじめワークスペースに設定されているため、追加のセットアップは必要ありません。Copilot がツールを初めて使うときに、VS Code で GitHub へのサインインを求められることがあります。画面の指示に従ってください。次の演習では、この接続の仕組みである Model Context Protocol について学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. Copilot に指示ファイルの更新に関する課題を探し、現在のブランチからプルリクエストを作成するよう依頼します。プルリクエストには、指示の更新と新しい出版社ヘルパーの基盤の両方を記載します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が GitHub の操作を実行することを許可するよう求められたら、コマンドを確認し、適切な場合は **Allow** を選択します。
5. プルリクエストが作成されたら、Copilot にマージを依頼し、ブランチを `main` に戻すために次のプロンプトを送信します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Copilot に GitHub 操作とシェル コマンドの実行を許可するよう求められたら、コマンドを確認し、適切な場合は **Allow** を選択します。

これで、GitHub Copilot を使ってプルリクエストを作成し、マージできました。

## まとめと次のステップ

この演習では、Copilot がプロジェクトの指示ファイルからコンテキストを取得する仕組みを確認しました。また、VS Code の Copilot Chat を使って次のことを行いました。

- コード生成プロンプトを送信し、*既存の*指示で Copilot が生成する内容を確認しました。
- リポジトリ全体に適用する新しい標準を `.github/copilot-instructions.md` に追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- 次の演習で利用するため、指示とヘルパーの基盤を `main` にコミットしました。

次は[エージェント モード][next-lesson]で指示を活用し、Copilot にコードベース全体にわたる新機能を追加してもらいましょう。

## リソース

- [GitHub Copilot のカスタマイズに関する指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
