---
title: "演習 1 - カスタム指示 (クラウドエージェント)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot クラウドエージェント →][next-lesson]

生成系の人工知能 (AI) を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストにアクセスできるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何をしてほしいかだけでなく、どのように実行してほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスを指定した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code やコマンドラインインターフェイス (CLI) のハーネスとは異なり、この演習ではプロンプトの「変更前と変更後」を比較しません。Copilot クラウドエージェントは GitHub Issue に対して非同期に動作するため、リアルタイムで並べて効果を示すのは困難です。このハーネスの後半で、クラウドエージェントが作成するプルリクエストをレビューするときに、指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データ層には必ずユニットテストを用意します。
- ユーザーインターフェイス (UI) はダークモードにし、モダンな印象にします。
- TypeScript Documentation (TSDoc) のドキュメントコメントを使って、コードにドキュメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、ここに挙げた開発慣行に沿ってタスクを実行するために必要な情報を Copilot に提供します。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストと希望を Copilot に伝えられます。これにより、より適切な提案やコードスニペットを得やすくなります。希望するコーディング規約、ライブラリ、コメントの形式を指定できます。リポジトリ全体に適用する指示も、特定のタスクやファイルの種類に適用する指示も作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に渡される単一の指示ファイルです。技術スタック、開発中の内容の概要、ベストプラクティスなど、Copilot に送るチャットや CLI リクエストの多くに関係するプロジェクトレベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に合わせて作成できます。TypeScript や Astro など特定の言語、または UI コンポーネントや新しいテストの作成などのタスクに関するガイドラインを記述できます。

> [!NOTE]
> 統合開発環境 (IDE) で作業する場合、指示ファイルは Copilot Chat でのコード生成にのみ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot クラウドエージェントは、リポジトリレベルの指示と、`applyTo` フロントマターを含む `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、ほかの規格の指示ファイルにも対応しています。[対応する指示ファイル][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法を詳しく説明することは、このワークショップの範囲外です。ただし、サンプルプロジェクトには一般的な方法を示す例があります。基本的な考え方は次のとおりです。

- `copilot-instructions.md` には、開発中の内容やプロジェクト構成、全体的なコーディング標準など、プロジェクトレベルの指針を記述します。
- `*.instructions.md` ファイルには、ファイルの種類 (ユニットテスト、Astro コンポーネント、データ層など) や特定のタスクに関する指針を記述します。
- 自然な言葉で、明確な指示と、望ましいコードや避けるべきコードの例を示します。

指示ファイルの作り方や AI の使い方に、唯一の正解はありません。プロジェクトで試しながら、最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトで、充実した指示ファイルを用意することをお勧めします。このプロジェクトのファイルを見ると、[UI の変更][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示があることが分かります。
>
> Copilot に指示ファイルを作成してもらうこともできます。各環境での操作方法は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init`)。該当する操作は、各環境のレッスンで説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認してください。基本の `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明に加えて、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の中にある **GitHub Actions Workflows** のガイダンスも確認します。これらは Copilot を使うあらゆる場面に適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示ファイルがあります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示を適用するファイルをリポジトリルートからの glob で指定します。この例では、`**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトでユニットテストを作成するための指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの別の指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。こうすると、大きな指示を再利用可能な小さなファイルに分け、Copilot がコードを生成するときに参照できる例を示せます。ここにあるパスは、リポジトリルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で、TSDoc のドキュメントコメントとファイルコメントヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

次に、この演習で行う唯一の編集として、コード内に TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーを追加する、プロジェクト全体のルールを設定します。後の演習で Issue を割り当てると、クラウドエージェントはこのルールに従います。

編集する前に、作業用のブランチを作成します。クラウドエージェントは Issue の対象ブランチにある指示を読み取りますが、ここでは変更を整理してコミットします。

1. codespace のターミナルで、次のコマンドを実行して新しいブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を伝えるためのファイルです。コード提案の質を高めるために、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントのルールはありません。そのため、生成されたヘルパーにはドキュメントコメントがありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加します。これにより、Copilot にファイルコメントヘッダーと TSDoc のドキュメントコメントを追加するよう指示します。

   ```markdown
   - エクスポートされたすべての関数に、目的、パラメーター、戻り値を説明する TSDoc コメントを付けます。
   - インポートやコードの前に、ファイルの目的を説明するコメントブロックを追加します。
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベルの `.github/copilot-instructions.md` に置いて全体に適用する方法と、言語、ファイルの種類、タスクに応じた `*.instructions.md` ファイルに記述する方法があります。ドキュメントコメントのルールのようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

クラウドエージェントは、Issue の対象ブランチにある指示ファイルを読み込みます。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、クラウドエージェントが指示を利用できるように、変更を `main` に反映する必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で Issue に割り当てる作業に、クラウドエージェントがこの指示を適用します。

> [!TIP]
> このワークショップで直接 `main` を使う場合は、ブランチの作成を省略して `main` に直接コミットできます。ここでは、実際のプロジェクトで行う方法に沿ってブランチを使います。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの後半で、クラウドエージェントが作成するプルリクエストを確認し、標準が適用されているかを確認します。

次は、[最初の Issue を Copilot クラウドエージェントに割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [指示ファイルやその他のリソースを集めた Awesome Copilot][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
