---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot をクラウドで非同期に動作させることができます。GitHub 上で作業を割り当てると、クラウドエージェントがバックグラウンドでリポジトリを調査し、変更を加え、プルリクエストを作成します。その間、開発者はほかの作業を続けられます。

各演習では、まずクラウドエージェントが従うカスタム指示を追加し、GitHub Issue を割り当てて実装を任せます。カスタムエージェントをレビューして使用し、エージェントのダッシュボードからセッションを監視して方向付けします。最後に、作成されたプルリクエストをレビューし、結果を反復して改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成します |
| [1. カスタム指示][ex1] | コンテキスト | クラウドエージェントが従うカスタム指示を追加します |
| [2. クラウドエージェント][ex2] | 非同期エージェント | GitHub Issue を Copilot クラウドエージェントに割り当てます |
| [3. カスタムエージェント][ex3] | 専門エージェント | カスタムエージェントをレビューして使用します |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視して方向付けします |
| [5. 反復して改善する][ex5] | レビュー | プルリクエストをレビューし、Copilot の作業を反復して改善し、次のステップを選びます |

## 前提条件

このワークショップを始める前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランのいずれかがある GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot クラウドエージェント

> [!TIP]
> 有料プランをお持ちでない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、モデルコンテキストプロトコル (MCP)、コードレビュー、Copilot コマンドラインインターフェイス (CLI) の機能が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> クラウドエージェントは **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free では利用できません。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP サーバーを使う方法を確認するには、[CLI ハーネス](../cli/)または [VS Code ハーネス](../vscode/)を参照してください。

## 始める

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
