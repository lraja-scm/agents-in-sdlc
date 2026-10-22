---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに `quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルター機能を確認しました。次は、明確な QA プロセスを持つカスタム エージェントにこれらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが命令、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な quality assurance (QA) プロファイルを作成して調べます。
- QA エージェントを選択し、フィルター機能の issue に照らした結果をレビューします。

## シナリオ

Tailspin Toys では、プル リクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザーでの動作を一貫して確認したいと考えています。カスタム エージェントを使うと、QA プロセスを調整し、再利用できるレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する特化型の Copilot です。プロファイルには、エージェントの目的、命令、利用可能なツールを記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

各カスタマイズには異なる役割があります。リポジトリの命令はチームの標準を示し、`quality-checks` スキルは繰り返し行うチェックをまとめ、Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、それらを使って要件を評価し、結果を報告する方法を Copilot に指示します。これらを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能 PR を作成する前に、QA が実施するチェックと守るべき境界を定義した、再利用可能なプロファイルを Copilot に作成してもらいます。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタム エージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が想定した QA ワークフローと境界を設定できたか確認します。これにより、検証だけを依頼したのに、プロファイルの範囲が不完全だったり広すぎたりする事態を防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は任意ですが、指定すると表示名が分かりやすくなります。
3. プロファイルの命令を読み、QA が要件を起点にして、リポジトリの命令に従い、`quality-checks` スキルと Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に許可を求め、変更をコミットしたり PR を開いたりしないことを確認します。
5. 必要な責任や制約が抜けていたら、続行する前に既定のエージェントへ修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクト エージェントを読み込みます。プロファイルを作成した後で同じフィルター機能の会話を再開し、QA を選択すると、issue と計画で決めた内容を使ってレビューできます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成直後は利用可能なエージェントの一覧に表示されない場合があります。上記のコマンドで有効にできます。

2. 次のプロンプトで QA エージェントに機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業する様子を確認します。
4. 処理が終わったら、レポートを読みます。

## まとめと次のステップ

再利用可能な専門家の役割を追加し、その作業をレビューしました。具体的には、次のことを行いました。

- カスタム エージェントが命令、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な QA プロファイルを作成して調べました。
- QA エージェントを選択し、フィルター機能の issue に照らした結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は[内容を機能 PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用カスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
