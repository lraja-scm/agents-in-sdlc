---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験を改善し、機能を追加するために、GitHub Copilot と複数のタスクに取り組みました。コードへのドキュメント追加、デザインチームが改善を重ねられる関連ゲーム機能の作成、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能の実装を依頼しました。ここでは、コードの変更を確認し、必要に応じて Copilot にフィードバックを送り、作業を改善します。その後、チームのほかのメンバーにもレビューを依頼します。

### シナリオ

これまで何度も確認したように、生成系の人工知能 (AI) を使ってもソフトウェア設計と DevOps の基本は変わりません。生成されたコードは必ずレビューし、通常の DevOps プロセスに沿って作業します。そこで、チームのレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot の提案を確認します。

## セキュリティと GitHub Copilot クラウドエージェント

Copilot クラウドエージェントは非同期かつ監視なしでタスクを実行するため、安全性を確保するためのセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、コードの作業に使うブランチにのみ書き込みアクセス権を持ちます。
- クラウドエージェントは GitHub Actions 内で実行され、作業用に独立した一時的な環境を作成します。
- GitHub Actions ワークフローは、人間が承認してから実行されます。
- [外部リソースへのアクセスは、デフォルトで制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。モデルコンテキストプロトコル (MCP) サーバーも対象です。

## 生成されたドキュメントをレビューする

まず、コードにドキュメントを追加するために GitHub Copilot クラウドエージェントが作成した最初のプルリクエスト (PR) を確認します。GitHub.com の標準的な PR 画面を使います。

> [!NOTE]
> PR を確認するときに、GitHub Copilot がファイアウォールでブロックされているという警告が表示される場合があります。これは想定どおりです。Copilot は、外部 MCP サーバーへの呼び出しを含め、デフォルトでは外部リソースへのアクセスが制限されています。必要に応じて、[Copilot クラウドエージェントのファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が表示されます。その場合は、作業が完了するまで待ちます。数分かかることがあるため、休憩したり、ここまでに学んだ内容を振り返ったりしてください。

4. プルリクエストの準備ができたら、**Files changed** タブを選択して変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく更新されたコードを確認します。TypeScript Documentation (TSDoc) のドキュメントコメントなどが追加されています。具体的な変更内容は異なります。

    変更を確認するときは、TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーを探します。これらは演習 2 の最初に確認したカスタム指示ファイルから適用されたものです。

6. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. 承認待ちのワークフローを示す表示があることを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェックセクションで、ワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するのは一方通行ではありません。チームメンバーと同じように、プルリクエストやコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確定的ではないため、どのような内容を依頼すべきか一律には指定できません。変更を依頼する例を次に示します。

- 各コードファイルの先頭に、ファイルの役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上の例のいずれか、またはコードベースのドキュメントに関して依頼したい変更をコメントに記入し、ほかのユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択して、Copilot が作業を進める様子を確認します。Copilot が更新のために新しいセッションを開始したことを確認します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

6. 納得できるまで、Copilot とのやり取りを続けます。
7. 変更に問題がなければ、ドラフトからレビュー準備完了に変更し、`main` ブランチにマージできます。

![PR をレビュー準備完了に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue について、Copilot が作成した PR に戻ります。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. `ゲーム詳細ページに関連ゲームを表示する` などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
6. 承認待ちのワークフローを示す表示があることを確認します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェックセクションで、ワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストすることもできます。codespace に移動し、ターミナルを開いて、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます。例: **copilot/fix-8**)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。先ほどと同じように、クラウドエージェントとのやり取りを続けて変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc のドキュメントコメントを追加したりできます。この Issue はカスタム指示を更新する**前**に割り当てたため、後者の指示は自動的に反映されていません。前と同様に、**Conversation** タブで新しいコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 のミッションコントロールで依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. `Web サイトにハイコントラストモードを追加する` などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が表示されます。その場合は、作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モードを切り替えるユーザーインターフェイス (UI) コンポーネント。
   - 利用者の設定を保存するローカルストレージの使用。
   - ハイコントラストモードとライトモードの Cascading Style Sheets (CSS) またはスタイルの変更。
   - アクセシビリティ属性 (Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作など)。
   - モードの切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. 承認待ちのワークフローを示す表示があることを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェックセクションで、ワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。codespace に移動し、ターミナルを開いて、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動して、ブラウザーでハイコントラストとライトモードの切り替えをテストします。期待どおり動作し、ページの再読み込み後も設定が保持されることを確認します。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに沿って Copilot の実装を導いたことを確認します。アクセシビリティ上の懸念や改善点があれば、前の PR と同様にコメントで **@copilot** をメンションして更新を依頼できます。

## 省略可能な演習 — 委任を続ける

クラウドエージェントには、実際のバックログ項目を任せ、その間に別の作業を進める方法が適しています。この方法に慣れるため、リポジトリでさらにいくつかの Issue を作成し、Copilot に割り当ててみましょう。たとえば、次のタスクを試せます。

1. ゲーム詳細ページに支援に関心のある人向けのフォームを作成します。
2. ゲーム一覧ページにページネーションを実装します。
3. データアクセスヘルパーに入力検証とエラー処理を追加します。

## まとめ

クラウドエージェントのハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- **リポジトリに含まれるカスタム指示ファイルを確認し、** 後でクラウドエージェントの出力にどう反映されるかを把握しました。
- **Issue を Copilot クラウドエージェントに割り当て、** 環境をセットアップし、計画を立て、非同期で作業する様子を確認しました。
- **アクセシビリティ用カスタムエージェントを作成して使用し、** ハイコントラストとライトモードの切り替えを追加しました。
- **Copilot エージェントページをミッションコントロールとして使用し、** アクセシビリティのセッションを監視して方向付けました。
- **クラウドエージェントのプルリクエストをレビューして改善し、** `@copilot` をメンションして変更を依頼し、ワークフローを承認しました。

## 振り返りと次のステップ

クラウドエージェントのハーネスを完了しました。ここで練習した内容をさらに広げたい場合は、ほかのハーネスで別の環境から関連するシナリオに取り組めます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — 統合開発環境 (IDE) から直接 Copilot Chat のエージェントモードと MCP 統合を確認します。
- 💻 **[コマンドラインインターフェイス (CLI) ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェントスキル、カスタム品質保証 (QA) エージェント、Playwright MCP、Agent Merge、省略可能な `/delegate` によるクラウドエージェントとの連携を使い、ターミナルから 2 つの変更をレビューして公開します。

自分のリポジトリで、次のアイデアを試すこともできます。

1. リファクタリングやテストカバレッジに関する Issue をクラウドエージェントに割り当てます。
2. 別の分野向けのカスタムエージェントを作成します。
3. 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot クラウドエージェントのファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
