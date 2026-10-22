---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときはコンテキストが重要です。特定の方法でタスクを実行したい場合、その指針を Copilot が参照できるようにします。[命令ファイル][instruction-files]には、作成するコードだけでなく、その構成方法も記述できます。フィルター機能を作成したので、Copilot が使用した命令を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの命令とパス別の命令ファイルがエージェントにどう渡されるかを確認します。
- コーディング標準に従うよう命令ファイルを更新します。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発方法に関するガイドラインがあります。

- コメントにはコードの言い換えではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を TSDoc/JSDoc で記述します。該当する場合は、注入可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントには `Props` の仕様を記述し、関連コードの変更に合わせてコメントを更新します。
- 既存の書式設定と lint の指針を維持します。

命令ファイルを使って Copilot に適切な情報を提供し、これらの開発慣行に沿うようにします。

## 命令ファイル

カスタム命令はコンテキストや希望を Copilot に伝え、コーディング スタイルや要件に沿った提案とコード生成を助けます。リポジトリ全体に適用する命令と、特定のファイル種別に適用する命令を作成できます。

このプロジェクトには、最初から命令ファイルが用意されています。変更する前に内容を確認し、命令の影響を把握します。

1. Codespace に戻ります。
2. Codespaces エディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング指針を確認します。このファイルの命令は、リポジトリに関するすべての Copilot とのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドは、命令を適用するファイルの glob を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルやプロジェクト内の既存ファイルへのリンクを確認します。命令を複数のファイルに分けて再利用できます。

## チームの指針に合わせて命令を更新する

既存のファイルにはまだ不足があります。コアの `copilot-instructions.md` に、生成する TypeScript に TSDoc コメントを追加する指針を設定します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します。35 行目付近にあります。
2. 次の項目をセクションの最後に追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新した指針を適用する

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。機能のコンテキストを保ったまま新しい指針を読み込めるよう、フィルターの会話を終了して再開します。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリのルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められたらフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新した指針をコードに適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が命令ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの命令とパス別の命令ファイルがエージェントにどう渡されるかを確認しました。
- 命令ファイルを更新して、コーディング標準を設定しました。
- 命令ファイルがコードに与える影響を確認しました。

次は[再利用可能な quality-checks スキルをカスタマイズして実行し、lint とテストを一貫して実行します][next-lesson]。

## リソース

- [Copilot CLI にカスタム命令を追加する][instruction-files]
- [カスタム命令のサポート][custom-instructions-support]
- [効果的なカスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
