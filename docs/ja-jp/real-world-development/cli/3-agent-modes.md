---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で issue からフィルター機能を実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作し、適切なものを適切な方法で構築できるよう支援します。このレッスンから、一般的なエージェント駆動の開発プロセスに沿って作業します。まず issue から機能を作り、コードの妥当性と期待どおりの動作を確認して、最終的にプロジェクトへマージします。

> [!NOTE]
> 機能開発の間は、同じ会話とブランチを使います。通常は作業するファイルの種類ごとにブランチや PR を分けますが、ここでは中心的な概念に集中するため、手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から Copilot の新しい会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送ブラウザーで機能を手動検証します。

この機能の作業を続けながら、リポジトリの命令を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加し、QA エージェントを作成して、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がカテゴリと発行元でゲームを絞り込める必要があります。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細があります。Plan モードで決定事項を整理し、範囲を限定した実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ基本が一層重要になります。多くの開発者は、次のような流れで作業します。

1. 作業内容を記述した issue から始めます。
2. 構築する内容の計画を作成します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プル リクエスト (PR) を作成します。
7. コードレビューが完了し、継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細な手順はチームや組織によって異なりますが、多くの場合はここに示した流れに沿っています。

この標準的な手順に沿うことで、AI が生成したコードも、手書きのコードと同じように要件に照らして確認し、検証してからマージできます。

## 会話モード

**会話モード**は、エージェントがどの程度自律的に動作するかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、続行前に入力を待つため、共同で作業できます。
- **Plan**: エージェントが計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認してから、Autopilot で実装します。

## issue から始める

フィルター機能に着手する前に、Codespace とリポジトリ、ターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新してから、フィルター機能のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` または同様の番号が表示されます。`#` を使うと、GitHub の issue やプル リクエスト (PR) を会話のコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、機能の実装やタスクを Copilot に任せる前に方針を決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan モードに切り替え、計画の作成を Copilot に依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した issue 参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査して、適切な方法を判断しながら計画を作成します。

3. フィルターの動作について質問された場合は、希望する内容に基づいて回答します。唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画をスクロールして確認します。
6. 自分の決定と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューできたら、Copilot に実装を任せます。Autopilot では、Copilot は完了したと判断するまで問題に取り組みます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンで同様のラベルが付いた選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。まず計画を確認してください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェック完了とみなす前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同じように、マージする前にレビューします。コードを確認し、サイトを実行して動作を確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** や **Autopilot** がないことを確認します。
2. `/diff` を入力し、フィルターの実装とテストを確認します。
3. 結果を issue および計画で決めた内容と照らし合わせます。
4. コードのレビューが終わったら、<kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードを確認したら、アプリを起動し、以前と同じように Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces にポート `4321` が使用可能と表示されたら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリ、発行元、および組み合わせで絞り込みます。
4. リセットと結果なしの場合の動作が、issue と合意した内容に沿っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

会話モードを切り替えて、機能を構築しレビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から Copilot の新しい会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送ブラウザーで機能を手動検証しました。

次は[カスタム命令を使ってコード生成をガイドします][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
