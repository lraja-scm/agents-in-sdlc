---
title: "演習 3 - Playwright の Model Context Protocol (MCP) サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認します。アプリを自分で操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開してプルリクエスト (PR) を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot を拡張する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にサーバーを使わせ、ブラウザーでフィルター機能を手動テストします。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にもアクセスできると、さらに便利になります。MCP は Copilot が外部と連携するための標準的な方法であり、エージェントが外部のツールやサービスと通信するために使います。

![エージェントモードとコンテキスト、大規模言語モデル (LLM)、ツールの連携を示す図。MCP サーバーと VS Code 拡張機能が提供するツールも含む](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、人工知能 (AI) エージェントが外部のツールやサービスと通信するための方法を提供します。MCP を使うと、AI エージェントはリアルタイムで外部のツールやサービスと通信できます。これにより、リソースから最新情報を取得したり、ツールを使って代わりに操作を実行したりできます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じてアクセスします。MCP サーバーは、既存のアプリケーションプログラミングインターフェイス (API) や npm パッケージなどのローカルツールと AI エージェント間の通信を管理します。各 MCP サーバーは、AI エージェントがアクセスできるツールとリソースを提供します。

よく使われている MCP サーバーを 2 つ紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理するための一連の API にアクセスできます。新しいリポジトリの作成、既存のリポジトリの更新、Issue やプルリクエストの管理などを AI エージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザーの自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などを AI エージェントが実行できます。

ほかにも、さまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub がホストする [MCP レジストリ](https://github.com/mcp)を使うと、エコシステム内のサーバーを見つけたり、コントリビューションを確認したりできます。

> [!CAUTION]
> セキュリティに関しては、MCP サーバーをほかの依存関係と同じように扱ってください。利用する前に、ソースコードを慎重にレビューし、発行元を確認して、セキュリティ上の影響を検討します。信頼できる MCP サーバーのみを使用し、機密リソースや重要な操作へのアクセスを許可するときは注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルでは、この VS Code ワークスペースで利用できる MCP サーバーを設定します。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトテンプレートに含まれています。そのため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使用するため、ローカルにインストールするものはありません。VS Code は Hypertext Transfer Protocol (HTTP) 経由で接続し、Copilot が初めてツールを使用するときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを利用できます。ここでは、このブラウザーを使って機能をテストします。

1. `.vscode/mcp.json` で、`github` と並ぶように `playwright` エントリを追加します。ファイルは次のようになります。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートサーバーの `github` とは異なり、`playwright` は**ローカル**サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、Playwright は画面を表示せずにブラウザーを実行します。codespace には画面を表示するデスクトップがないため、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトは、エンドツーエンドテストに Playwright をすでに使用しているため、必要なブラウザーは通常インストール済みです。Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行して、もう一度試すよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要になったときに**起動します。Copilot が初めてサーバーのツールを必要とするときに、VS Code がサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。これを選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、設定を確認し、サーバーを起動できるよう信頼することを選択します。

手動で起動する必要はありません。ここで起動しない場合は、次の手順で Copilot が初めてツールを使うときに、VS Code がサーバーを起動して信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。開発者がアプリを自分で操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して結果を読み取り、その内容をまとめます。会話から離れずに、機能が期待どおり動作するかをすばやく確認できます。

Playwright MCP サーバーは、スクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使います。エージェントは、支援技術と同じように、ボタン、リンク、リスト項目などラベル付きの構造化された要素を使って処理します。そのため、簡単な機能テストをアクセシビリティの基本的な確認にも活用できます。

サーバーを接続し、アプリが起動していることを確認してから、次の手順で Copilot にフィルター機能をテストさせます。

1. アプリが `http://localhost:4321` で実行されていることを確認します。開発サーバーを停止した場合は、[前の演習](../2-agent-mode/)で使った `npm run dev` を実行して再起動します。
2. Copilot に次のプロンプトを送信します。

    ```text
    Playwright MCP サーバーを使って、実行中のアプリ (http://localhost:4321) をブラウザーで開き、新しいゲームの絞り込み機能を検証してください。

    1. ゲームページを開き、一覧に表示されるゲームの数を確認します。
    2. カテゴリフィルターを適用し、そのカテゴリのゲームだけが一覧に表示されることを確認します。
    3. フィルターをクリアし、パブリッシャーフィルターを適用して、そのパブリッシャーのゲームだけが一覧に表示されることを確認します。
    4. カテゴリとパブリッシャーの両方のフィルターを適用し、両方の条件を反映した結果になることを確認します。

    各手順で確認した結果を報告し、期待どおりに動作しない点があれば指摘してください。
    ```

3. Playwright がブラウザーが見つからないと報告した場合は、Copilot に `npx playwright install chromium` を実行してもう一度試すよう依頼します。

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。その要約を Issue の受け入れ条件と照らして確認します。問題があれば、プルリクエストを開く前に追加で質問するか、コードの修正を依頼します。

> [!NOTE]
> Copilot が初めて Playwright MCP サーバーを使うときに、ブラウザーをダウンロードする場合があります。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームのレビュー用にプルリクエスト (PR) を作成します。まず、`filtering-vscode` ブランチを公開します。

1. codespace で **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して、変更をステージします。
3. **Sparkle** ボタンを選択して、コミットメッセージを生成します。

   ![変更を表示した Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、リポジトリにブランチをプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) を使うなど、プルリクエストの作成方法はいくつかあります。ここでは GitHub Copilot を使って、関連 Issue を見つけ、Issue と関連付けた PR を作成します。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェントのドロップダウンで **Agent** が選択されていることを確認します。これにより、Copilot が GitHub ツールを使えるようになります。
3. 次のプロンプトで、PR の作成を Copilot に依頼します。

   ```plaintext
   カテゴリとパブリッシャーによる絞り込みに関連する Issue をリポジトリ内で探してください。現在のブランチのプルリクエストを作成し、該当する Issue と関連付けてください。
   ```

4. Copilot が情報の収集や操作を行うために必要に応じて **Continue** を選択します。GitHub ツールを初めて使うときは、GitHub での認証を求められる場合があります。画面の指示に従って許可します。
5. Copilot が Issue を検索して該当するものを見つけ、PR を作成する様子を確認します。
6. Copilot が作成したリンクを選択してプルリクエストを確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが新しいツールで Copilot を拡張する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーで機能を公開する前に、サーバーを使ってフィルター機能を手動テストしました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

機能の**テスト**に MCP サーバーを使いましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は、アクセシビリティレビューなど、対象を絞ったタスクを効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP レジストリ][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
