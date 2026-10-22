---
title: "演習 2 - GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどが考えられます。GitHub Copilot のクラウドエージェントは、コードの更新や機能追加などを自律的に実行します。作業が終わると、人間の開発者がレビューできるドラフトのプルリクエストを作成します。手間のかかる作業を任せることで開発を加速し、開発者はより大きな課題に集中できます。

Copilot クラウドエージェントを使って、次のことを確認します。

- コード生成環境をカスタマイズする方法。
- 操作を安全に実行する方法。
- Issue の範囲を明確にする重要性。
- Issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には、解消したい技術的負債があります。サイトの初期バージョンを作成した開発者は、ドキュメントを十分に整備しませんでした。具体的には、ドキュメントがほとんどありません。まず、アプリケーション内のエクスポートされたすべての関数に TypeScript Documentation (TSDoc) のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリのほかのタイトルを「関連ゲーム」として表示し、閲覧を続けられるようにします。まだ洗練された実装は必要なく、ユーザー体験の受け入れテストに使える程度で構いません。現在は作業の妨げになっていますが、ほかに優先度の高い Issue があります。

どちらも優先度が下がりやすいタスクの例です。Copilot クラウドエージェントに割り当てると、バックグラウンドで非同期に作業できます。その間、開発者は別のタスクに集中し、後から戻って作業内容をレビューできます。

## GitHub Copilot クラウドエージェントの概要

[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様に、バックグラウンドでタスクを実行できます。人間の開発者に作業を依頼するのと同じく、[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、複数の方法で依頼できます。割り当てると、Copilot は進捗を記録するドラフトのプルリクエストを作成し、環境をセットアップしてタスクに取りかかります。作業中でも完了後でも、Copilot のセッションを確認できます。提案された解決策をレビューできる状態になると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法のような力はありません。考える手間を省けるわけではありません。一見簡単な作業でも、詳しく見ていくと複雑さがあるため、[クラウドエージェントに依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。人工知能 (AI) を活用するペアプログラマーとして扱い、段階を踏んで学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示と、それが Copilot を導く仕組みを確認しました。カスタム指示の演習をすでに完了している場合は、`.github/copilot-instructions.md` の効果を確認しています。まだの場合は、ここで簡単に見ておきましょう。Copilot クラウドエージェントに作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認しておくと、後で影響を見つけやすくなります。

カスタム指示の内容を確認するため、次の手順でリポジトリのファイルを開きます。

1. リポジトリの GitHub Web ユーザーインターフェイス (UI)、または起動済みの codespace で `.github/copilot-instructions.md` を開きます。**Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を確認します。
2. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` フロントマターが `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで `コードのドキュメントが不足している` Issue をクラウドエージェントに割り当てるときに、作成されたプルリクエストに TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーが反映されているかを確認してください。後の演習でプルリクエストをレビューするときに、もう一度取り上げます。

## Copilot クラウドエージェント用の開発環境をセットアップする

コードの作成には、誰が作業する場合でも通常、特定の環境と、正常な状態にするためのセットアップスクリプトが必要です。同様に、ソフトウェアエンジニア (SWE) のように動作する Copilot にタスクを割り当てる場合も、環境を用意します。

クラウドエージェントは、作業時の環境として [GitHub Actions][github-actions] を使います。作業の開始前に実行される[専用のセットアップワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成すると、必要な開発ツールや依存関係を利用できるよう環境をカスタマイズできます。ラボをスムーズに進め、学習できるように、この設定はあらかじめ用意されています。単一の Astro アプリ用に、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシード済みの SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 独自の手順に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # たとえば依存関係をインストールするために、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリは Astro + Drizzle/libSQL のみになりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテスト用にローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローファイルと同じように見えますが、重要な点がいくつかあります。

- `copilot-setup-steps` という 1 つのジョブがあります。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot が実行するのを待たずに、正常に動作するかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は誰もが理解していますが、多くのプロジェクトでは情報が古いか、そもそもドキュメントがありません。このような技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成を得意とするため、この作業をクラウドエージェントに依頼するのに適しています。バックグラウンドで必要なドキュメントを生成します。後の演習で作業内容をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `コードのドキュメントが不足している` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    組織では、必要に応じて関数やメソッドに TSDoc のドキュメントコメントを含めることが求められています。しかし、最近の更新ではこの標準に従っていません。動作を明確にする箇所にドキュメントコメントを含めるよう、既存のコードを更新する必要があります。
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

  ![Copilot の割り当ての詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: コードのドキュメントが不足している` のようなものです。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が To Do リストを作成したことを確認します。

> [!NOTE]
> Copilot の To Do リストが PR に表示されるまで、数分かかる場合があります。Copilot は、前述のワークフローを実行して環境を作成し、プロジェクトを分析して、問題に取り組む方法を検討しています。

13. リストを確認し、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッション画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された状態が表示されるまで、ウィンドウの更新が必要な場合があります。

16. ライブセッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調査して状態を把握し、計画を検討するために一時停止し、適切な方法を決めてコードを作成する過程を確認できます。

作業には数分かかる場合があります。Copilot クラウドエージェントの主な目的の 1 つは、ほかの作業に集中できるように、非同期でタスクを実行することです。この機能を活用して、別の作業を進めながら、もう 1 つのタスクをクラウドエージェントに割り当てます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

前述のとおり、GitHub Copilot クラウドエージェントの大きな利点の 1 つは、作業を分担できることです。デザインチーム向けに関連ゲームのセクションを追加する作業に時間がかからないとしても、その間にほかの作業を進められます。このタスクも Copilot クラウドエージェントに割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `ゲーム詳細ページに関連ゲームを表示する` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    ゲーム詳細ページに関連ゲームを表示して、利用者がより多くのゲームを見つけられるようにします。デザインチームはユーザー体験を検討し、受け入れテストを行いたいと考えています。要件は次のとおりです。

   - `src/lib/` にデータアクセスヘルパーを追加し、指定されたゲームを除く、同じカテゴリのほかのゲームを返す
   - ヘルパーを使って、ゲーム詳細ページに "Related games" セクションを表示する
   - 関連ゲームがない場合にも対応する
   - 新しいヘルパーのユニットテストを作成する
   - プルリクエスト (PR) を作成する前に、すべてのテストが成功することを確認する
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示されます。Copilot が作業中であることを示します。

![Issue で Copilot の作業中を示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot クラウドエージェントは SWE と同じように動作するため、常に監視する必要はありません。作業が完了してからレビューできます。続いて、カスタムエージェントの作成と使用方法を確認します。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot クラウドエージェント][copilot-agents]を確認しました。クラウドエージェントに Issue を割り当てて、非同期で作業させることができます。技術的負債の解消、新機能の作成、あるフレームワークから別のフレームワークへのコード移行などに活用できます。

次の内容を確認しました。

- コード生成環境のカスタマイズ。
- 操作を安全に実行する方法。
- Issue の範囲を明確にする重要性。
- Issue を Copilot に割り当てる方法。

クラウドエージェントがバックグラウンドで作業している間に、次はカスタムエージェントの作成と使用に進みます。[Copilot クラウドエージェントはモデルコンテキストプロトコル (MCP) サーバーも利用できます][cloud-agent-mcp]。また、前の演習で確認したカスタム指示も利用できます。

## リソース

- [Copilot クラウドエージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
