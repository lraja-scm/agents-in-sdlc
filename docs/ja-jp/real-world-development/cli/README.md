---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プル リクエストの自動化を学びながら、Tailspin Toys に 2 つの変更を加えて検証し、提供します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、ターミナルでエージェント型コーディング アシスタントとして GitHub Copilot を利用できます。コードベースの調査、コードの生成、コマンドの実行、外部ツールへの接続をコマンド ラインから行えるため、グラフィカル エディターに切り替えずに作業を続けられます。

このワークショップでは、GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして使い方を確認します。まず星評価の小さな変更を加え、ブラウザーで確認して、最初のプル リクエストを手動でマージします。次にフィルター機能の issue を起点に、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューします。リポジトリの命令を更新し、既存の `quality-checks` スキルをカスタマイズしてプロジェクトのチェックを実行します。Playwright Model Context Protocol (MCP) サーバーでブラウザー上の動作を調べ、quality assurance (QA) カスタム エージェントで要件、テスト範囲、検証結果を確認します。最後に、フィルター機能の変更をレビューして Agent Merge を使用し、コンテキスト、モデル、共有、クラウドへの任意の委任に役立つ slash command を確認します。

ワークショップを進めやすくするため、プル リクエスト (PR) は星評価と、命令の更新、スキルの変更、QA プロファイル、テストを含むフィルター機能の 2 つにまとめます。フィルター機能の作業は同じ会話とブランチで続けるため、各ツールを学びながら変更を積み上げられます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----------- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI をインストールする][ex1] | インストール | Copilot CLI をインストールして認証し、使い方を確認する |
| [2. 星評価を追加する][ex2] | 最初の変更 | 既存の評価と未評価時の表示をカードに追加して、最初の PR をマージする |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | issue から機能を計画し、Autopilot で実装して Interactive モードでレビューする |
| [4. カスタム命令で Copilot を導く][ex4] | コンテキスト | 命令を確認、更新し、フィルター機能に適用する |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存のスキルを確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を構成し、フィルターの動作を調べる |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門プロファイルを作成して選択し、最終検証の根拠を集める |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI の slash command][ex9] | CLI の機能 | コンテキスト、モデル、共有、クラウド エージェントへの任意の委任を確認する |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、関連リソースを振り返る |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログ情報に基づく Backer Concierge を構築してデプロイし、Web サイトに接続する |

## 前提条件

このワークショップを始める前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、または Enterprise** プランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンド ラインの基本的な知識

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用する場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
