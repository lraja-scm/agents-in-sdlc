---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター機能、命令、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、命令の更新、スキルの更新、quality assurance (QA) プロファイル、テストを 1 つのブランチに保存しました。まとめてレビューし、プル リクエストを作成します。星評価の PR は自分でマージしましたが、今回は **Agent Merge** にマージのライフサイクルを管理させます。

> [!NOTE]
> 通常は機能、命令の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、フィルター機能と品質ワークフローを 1 つの会話とブランチで進め、1 つの PR にまとめます。

このレッスンでは、次のことを行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びます。
- 機能全体の変更と検証の根拠を調べます。
- フィルター機能の PR を作成します。
- レビュー後に限って Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージを許可する権限を開発者の手元に残しつつ、PR に関する残りの作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** は、プル リクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR の阻害要因に対処します。継続的インテグレーション (CI) チェックの失敗を修正し、レビュー コメントに対応し、必要に応じてリベースします。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せられますが、有効にする前に内容をレビューして、準備ができていることを確認してください。

## Agent Merge で PR を管理する

作成したコードをまとめてレビューし、PR を作成して、残りの処理を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功すると、マージが実行されます。
7. Copilot が `PR #14 was squash-merged successfully.` などのメッセージを表示したことを確認します。

> [!IMPORTANT]
> Agent Merge は必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限の不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている方法でレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プル リクエストの処理を自動化しました。具体的には、次のことを行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証の根拠を調べました。
- フィルター機能の PR を作成しました。
- レビュー後に限って Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、クラウドへの任意の委任に使える[便利な Copilot CLI の slash command を確認します][next-lesson]。

## リソース

- [Copilot CLI でプル リクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
