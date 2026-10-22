---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプル リクエスト、再利用可能なカスタマイズ、関連リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローを進めました。具体的には、次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを調べ、登録済みのフィルター機能の issue を確認しました。
- 星評価を追加し、転送ブラウザーで結果を確認して、最初のプル リクエスト (PR) を手動でマージしました。
- フィルター機能の issue から始め、Plan モードで方針を定義し、Autopilot モードで実装して、Interactive モードでレビューしました。
- カスタム命令でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を調べました。
- quality assurance (QA) カスタム エージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの根拠を評価しました。
- フィルター機能全体の変更をレビューし、その PR で Agent Merge を許可しました。
- コンテキスト、モデル、共有、クラウドへの任意の委任に使える slash command を確認しました。

## 作成したもの

更新した `main` から作成した個別のブランチで、2 つの PR を作成しました。

1. **星評価:** 既存の `starRating` と、未評価状態の表示をゲームカードに追加しました。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、命令を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルと関連テストを追加しました。

フィルター機能の計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを進めやすくするため、この作業を 1 つの PR にまとめました。

## 検証方法の違い

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー確認という複数の方法でコードを検証しました。`quality-checks` スキルは、単体テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は要件とテスト範囲のレビューに結果を加えて、情報をまとめました。

追加するテストは、実際に不足している範囲を補う必要があります。QA の実行で新しいテストが不要という結果になる場合もあります。マージを許可する前にコードと根拠を確認し、変更後は影響を受ける根拠を再度確認してください。

## ベスト プラクティス

Copilot に与えるコンテキストやツールが、結果に影響します。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを構成しました。会話をまたいでこれらのカスタマイズを再利用し、チームのニーズに合わせて調整してください。命令は標準を示し、スキルは繰り返し実行するタスクを記述し、カスタム エージェントは専門の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけではなく、実際の変更とツールの結果を確認してください。

タスクに合わせて **モードとモデル** を選びます。構築前に方針を考えるには **Plan**、範囲を絞った変更でやりとりを続けるには **Interactive**、範囲が明確なタスクを任せるには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を、なぜ、どのように構築するかを明確に伝えると、出力が大きく変わります。

## さらに試す

中心的なワークフローを学びました。Copilot CLI には、次の機能もあります。

- `/review` でコード レビュー エージェントに変更の分析を依頼できます。
- `/rubber-duck` で問題について話し合い、別の視点を得られます。
- `/fleet` で独立したサブタスクを並列に進められます。
- `/worktree` で別のタスクを分離できます。
- `/delegate` で Copilot cloud agent にタスクを送信できます。

## 次のステップ

どのツールも、使い続けることで上達します。本番コードや個人のコードに使ったり、長年作ろうと思っていた小さなアプリを作ったりしてください。チーム内で知見を共有し、ほかのメンバーからも学びましょう。必要に応じてドキュメントも確認してください。

ターミナルから Tailspin Toys を発展させる場合は、オプションの[Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
