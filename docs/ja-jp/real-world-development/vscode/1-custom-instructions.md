---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成系の人工知能 (AI) を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストにアクセスできるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。中でも重要なのが[指示ファイル][instruction-files]です。指示ファイルを使うと、コード生成の進め方を Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスを指定した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 現在の指示を適用したまま、フィルター機能のデータスライスの最初の部分 (パブリッシャー用ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成されたコードに新しい標準が適用される様子を確認します。
- 指示の更新とフィルター機能の一部を `main` にコミットし、次の演習で Copilot が更新済みの指針を使えるようにします。

> [!CAUTION]
> 生成されたコードが設定した標準と異なる場合があります。Copilot の出力は確定的ではありません。この演習の目的は、指示を更新した後の動作の「傾向」を確認することです。出力の文字単位の一致を目指すものではありません。

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

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に渡される単一の指示ファイルです。技術スタック、開発中の内容の概要、ベストプラクティスなど、Copilot に送るチャットやコマンドラインインターフェイス (CLI) のリクエストの多くに関係するプロジェクトレベルの情報を記述します。
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

## 変更用のブランチを作成する

ここではベストプラクティスに沿って、作業用のブランチを作成します。

1. 前の演習で使用した codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドを実行して、新しいブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> この演習では Copilot Chat の新しい会話を使います。モードとモデルの選択は演習によって異なるため、必要な箇所で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを適用した状態でプロンプトを送ります。その後、指示を更新し、同じプロンプトを再送して違いを確認します。

1. codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。
2. 前の演習で開いたエディターのタブをすべて閉じ、Copilot が必要なコンテキストだけを取得する状態にします。
3. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
4. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

5. 次のプロンプトを送信します。

   ```plaintext
   `src/lib/publishers.ts` を作成または更新し、各パブリッシャーの `name` と `id` を含む一覧を返すデータアクセスヘルパーを追加してください。ファイルの変更を適用してください。
   ```

6. Copilot がプロジェクトを調査し、ヘルパーファイルとテストなど、複数のファイルにコードの変更を適用します。
7. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取り、パブリッシャーの型付き配列を返す型付き関数であることを確認します。これは、`src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層の規約に従っています。
8. 提案されたコードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合も問題ありません。指示の更新後に、より一貫して標準が適用されることが重要です。

## リポジトリ全体に適用する標準を追加する

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

## プロンプトを再実行して変更を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話の内容をクリアします。
2. `src/lib/publishers.ts` を再び選択し、Copilot が対象ファイルに集中できるようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   `src/lib/publishers.ts` を作成または更新し、各パブリッシャーの `name` と `id` を含む一覧を返すデータアクセスヘルパーを追加してください。ファイルの変更を適用してください。
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディングプラットフォームのパブリッシャーデータアクセスヘルパーです。
    * データベースからパブリッシャー情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * ID と名前を含む、すべてのパブリッシャーの一覧を返します。
    *
    * @param db - Drizzle データベースクライアント。
    * @returns パブリッシャーオブジェクトの配列に解決される Promise。
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で発展させる実際のコードに適用しました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもリポジトリのほかのファイルと同様に、ソース管理で管理します。更新をコミットして、リポジトリにブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで、指示の更新とヘルパーの基礎実装に関して Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加ファイルが表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからリポジトリにブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、プルリクエストを作成し、バックログにあるドキュメント標準の Issue と関連付けます。手動で作成することもできますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に依頼します。Issue を特定し、解決する PR を作成して、マージするように依頼します。

> [!NOTE]
> この演習では初めて、Copilot が GitHub 上で操作します。プロジェクトのテンプレートには GitHub のツールがあらかじめ設定されているため、セットアップは不要です。Copilot が初めてツールを使うとき、VS Code から GitHub へのサインインを求められる場合があります。画面の指示に従って許可します。この接続の仕組みである Model Context Protocol (MCP) については、後の演習で説明します。

1. codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチからプルリクエスト (PR) を作成するよう Copilot に依頼します。PR には、指示の更新と、今後のフィルター作業に向けたパブリッシャー用ヘルパーの基礎実装の両方を記載します。

   ```
   指示ファイルの更新に関する Issue を見つけてください。現在のブランチから新しいプルリクエストを作成し、その Issue を解決することを明記してください。また、今後のフィルター作業に向けてパブリッシャー用ヘルパーの基礎実装も追加したことを記載してください。
   ```

4. Copilot が Issue を探し、PR を作成します。
5. Copilot が代わりに GitHub 操作を行うことを許可するよう求められたら、コマンドを確認して適切なものを **Allow** します。
6. PR が作成されたら、次のプロンプトで PR をマージし、ローカルのブランチを `main` に戻すよう Copilot に依頼します。

   ```
   PR を main にマージしてください。その後、ローカルで main に戻り、最新のコードを pull して最新状態にしてください。
   ```

7. Copilot が代わりに GitHub 操作とシェルコマンドを実行することを許可するよう求められたら、コマンドを確認して適切なものを **Allow** します。

GitHub Copilot の支援で PR を作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認しました。
- 既存の指示を適用した状態でパブリッシャー用ヘルパーを生成するプロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が適用される様子を確認しました。
- 指示の更新とヘルパーの基礎実装を `main` にコミットし、次の演習で利用できるようにしました。

次は、Copilot がコードベース全体に新機能を追加する[エージェントモード][next-lesson]で、指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [指示ファイルやその他のリソースを集めた Awesome Copilot][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
