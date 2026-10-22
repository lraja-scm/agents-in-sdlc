---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、人工知能 (AI) を活用した専用のアシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、タスクに応じた指示、ベストプラクティス、コーディングパターン、分野固有の知識を定義します。これにより、Copilot が特定の種類の作業をより効果的に行えるようになります。チームは専門知識を再利用可能なエージェントとしてまとめられます。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディング慣行に従うセキュリティエージェント、一貫したテストパターンを保つテストエージェントを作成できます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー、または共通の `~/.copilot/agents` に置いた Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML Ain't Markup Language (YAML) フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、論理的に重なる部分があります。どちらも主に Markdown ファイルで定義され、AI に作業方法を伝えます。簡潔に区別すると、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一部としてスキルや別のエージェントを調整できます。このラボでは、アクセシビリティ用カスタムエージェントがアクセシビリティガイドラインに沿ってサイトをレビューし、更新します。その作業の中で、プルリクエストのワークフローを扱うスキルや、テストを実行して管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI のほかの用途と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディングプラットフォームを使えるように取り組んでいます。利用者から、テキストと背景色のコントラストが十分でないため、現在のダークテーマでは読みづらいという意見が寄せられました。このアクセシビリティの課題に対処するため、デザインチームは利用者が切り替えられるハイコントラストモードを求めています。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。カスタムエージェントを使って、この機能を生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ用カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは、すでに用意されています。Copilot を導く方法を理解するため、内容を確認します。

1. codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用の新しいブランチを作成します。演習 3 のフィルタリングのプルリクエスト (PR) は別に管理します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必須です。

4. その下のセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイトのコードを生成するときの主な責務。
   - アクセシビリティのベストプラクティス。
   - HyperText Markup Language (HTML)、Cascading Style Sheets (CSS)、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code では、`.github/agents` で定義されたすべてのカスタムエージェントが、Copilot Chat ビュー下部のエージェントのドロップダウンに表示されます。カスタムエージェントを選択すると、その指示とツールに沿ったチャットセッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> この演習では Copilot Chat の新しい会話を使います。モードとモデルの選択は演習によって異なるため、必要な箇所で説明します。

1. codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。
2. Chat ビューのエージェントのドロップダウンで **Agent** が選択されていない場合は、選択します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

3. チャットビュー下部にあるエージェントのドロップダウンを選択します。ここには、現在のエージェントが表示されます。デフォルトでは **default** です。
4. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
5. アクセシビリティエージェントに次のプロンプトを送信します。

    ```plaintext
    サイトにハイコントラストモードを追加してください。利用者がハイコントラストモードを切り替えられるようにし、設定をブラウザーのローカルストレージに保存して、ページを再読み込みした後も保持してください。
    ```

6. Copilot Chat がコードベースを調査し、変更を提案して、プロジェクトファイルを編集します。各編集はファイルパスと差分とともにチャット内に表示されます。
7. 編集が適用されると、エージェントが変更した作業ファイルを示す **Files changed** の表示が更新されます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot はリポジトリに実際の変更を加えています。作業中に Astro コンポーネント、CSS、関連するテストなどの既存ファイルを編集します。

次の演習では、進行中の作業をレビューして方向付けます。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた専用の AI アシスタントである、GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot が特定の種類の作業をより効果的に行えるよう導けます。

次のことを行いました。

- カスタムエージェントの定義方法を確認しました。
- Copilot Chat のエージェントモードでカスタムエージェントを使いました。

次は、エージェントの作業を[監視して方向付けます][next-lesson]。変更を確認しながら、同じセッションにライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [統合開発環境 (IDE) でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright の Model Context Protocol (MCP) サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
