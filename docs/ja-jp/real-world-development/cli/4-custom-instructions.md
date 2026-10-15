---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント作成の標準を追加して、フィルターのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを進める必要がある場合は、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]には、希望するコードだけでなく、コードの構成方法も記述します。フィルターを実装したので、Copilot が使用した指示を確認し、ドキュメント作成の標準を追加して、コードに適用しましょう。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに届く仕組みを確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発のガイドラインと要件があります。

- コメントにはコードの繰り返しではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。該当する場合は注入可能な `db` 引数も説明します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を記述し、関連コードの変更時にはコメントも最新に保ちます。
- 既存のフォーマットと lint の指針を維持します。

指示ファイルを使って、Copilot がこれらのプラクティスに沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示を使うと、コンテキストや好みを Copilot に提供し、コーディング スタイルや要件を理解しやすくできます。コーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定のファイルの種類に適用する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリへの**すべての**リクエストで Copilot に送られる単一の指示ファイルです。多くのリクエストに関連するプロジェクト全体の情報を記述します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイル形式、タスク向けのガイドラインを記述します。

> [!NOTE]
> ほかの指示形式とサポート状況は環境によって異なります。特定の形式を使う前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すでに指示ファイルが含まれています。変更前に内容を確認して、影響を確かめましょう。

1. Codespace に戻ります。
2. Codespaces エディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この指示は、このリポジトリでのすべての Copilot とのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これは指示が適用されるファイルを決める glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクト ファイルへのリンクを確認します。大きな指示セットを再利用可能な小さなファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルには、TypeScript で新たに生成するコードに TSDoc コメントを追加する指示が不足しています。`copilot-instructions.md` を更新しましょう。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - 新しい TypeScript コードには、ドキュメントとして TSDoc コメントを記述します。
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。機能のコンテキストを保ちながら新しい指示を読み込むには、フィルターの会話を終了して再開します。

1. フィルターの会話がまだ開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   求められたらフィルターの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトで、更新した指示を適用するよう依頼します。

   ```plaintext
   指示とコード ガイダンスを更新しました。生成したコードを、このガイダンスに合わせて更新してください。
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新たに作成された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示を読み込む仕組みを確認し、機能に新しい標準を適用しました。次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実施できるようにします。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
