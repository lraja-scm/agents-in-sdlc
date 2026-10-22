---
title: "レッスン 9 - GitHub Copilot CLI の slash command"
description: "コンテキスト管理、モデル選択、セッション共有、クラウドへの任意の委任に使える slash command を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

一般的な CLI ツールと同様に、GitHub Copilot CLI にも操作用の slash command が多数あります。高度な機能や内部情報、追加の構成オプションを利用できます。すでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で Copilot の会話コンテキストの管理方法を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の中心的なワークフローが完了しました。次は、コンテキスト管理、モデルの切り替え、セッション共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキスト ウィンドウに達する場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、slash command を使って状態を確認したり、自分で圧縮したりできます。

1. Codespace に戻ります。まだ Copilot CLI を開いていない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム命令、ツール、メッセージ、空き容量ごとのコンテキスト配分を確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話がすでに短い場合、大きな変化がないこともあります。

> [!NOTE]
> コンテキスト ウィンドウがいっぱいになると、Copilot CLI は自動的にコンテキストを圧縮します。圧縮するタイミングを自分で決める場合は `/compact` を使います。無関係なタスクに切り替えて会話を新しくする場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意なことが異なり、開発者によって好みも違います。Copilot CLI ではモデルを一覧表示して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選択します。一覧を閉じる場合は <kbd>Esc</kbd> を押します。

## セッションを共有する

チームで協力し、学んだことを共有すると、AI ツールの活用方法を改善できます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みバージョンで利用できる `/share` のオプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown エクスポートに使う `/share file` など、目的に合った方法を選びます。
3. エクスポートした内容を、送信または公開する前に確認します。セッションのエクスポートにはプロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## オプション: 並列作業または委任

ワークショップの中心的な内容は完了しました。大きなタスクには、次の Copilot CLI コマンドも利用できます。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並列で実行できます。
- `/worktree` は、別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は、タスクを Copilot cloud agent に送り、非同期で作業させることができます。エージェントがプル リクエストを作成する場合もあります。

これらのコマンドは worktree やリモートの作業を追加することがあるため、任意です。試す前に、範囲を明確にした新しいタスクを開始し、通常のワークフローで結果をレビューしてください。非同期エージェントの作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI の slash command を使うと、設定を変更し、セッションを共有し、内部の状態を確認できます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で Copilot の会話コンテキストの管理方法を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI には、ほかにも多くの slash command があります。次は[学んだ内容と今後のステップを振り返ります][next-lesson]。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
