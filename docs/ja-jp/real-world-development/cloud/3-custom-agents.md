---
title: "演習 3 - カスタムエージェント"
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

カスタムエージェントを使って、次のことを確認します。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスクの割り当て方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディングプラットフォームを使えるように取り組んでいます。利用者から、テキストと背景色のコントラストが十分でないため、現在のダークテーマでは読みづらいという意見が寄せられました。このアクセシビリティの課題に対処するため、デザインチームは利用者が切り替えられるハイコントラストモードを求めています。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。カスタムエージェントを使って、この機能を生成します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは、すでに用意されています。Copilot を導く方法を理解するため、codespace で次の手順を行い、内容を確認します。

1. codespace に戻り、`.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必須です。

3. その下のセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイトのコードを生成するときの主な責務。
   - アクセシビリティのベストプラクティス。
   - HyperText Markup Language (HTML)、Cascading Style Sheets (CSS)、JavaScript のコード例。
   - よくある問題や間違い。

## Issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを管理する中心的な場所です。Copilot クラウドエージェントにタスクを割り当てたり、進行状況を監視したり、追加の指示を与えて方向付けたりできます。まず、ハイコントラストモードの作成を Copilot に依頼します。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Web サイトにハイコントラストモードを追加する` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    サイトにハイコントラストモードが必要です。利用者がハイコントラストモードを切り替えられるようにし、設定をブラウザーのローカルストレージに保存してください。
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![カスタムエージェントとして Accessibility agent が選択されている、クラウドエージェントの割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクに取りかかります。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた専用の AI アシスタントである、GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot が特定の種類の作業をより効果的に行えるよう導けます。

次の内容を確認しました。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスクの割り当て方法。

Copilot がハイコントラストモードを実装している間に、ミッションコントロールから[エージェントセッションを監視して方向付けます][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot クラウドエージェント][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
