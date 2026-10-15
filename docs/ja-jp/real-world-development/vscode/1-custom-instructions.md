---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを進める必要がある場合や、Copilot が知っておくべき背景情報がある場合は、参照できるようにしておきます。Copilot と特定のコンテキストを共有する方法はいくつかあります。特に重要なのが[指示ファイル][instruction-files]です。指示ファイルを使って、コード生成のガイダンスを提供できます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示を使って、フィルター機能の最初のデータ処理部分 (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再び使い、生成されるコードが新しい標準を採用する様子を確認します。
- 指示の更新とフィルターの一部を `main` にコミットし、次の演習で Copilot が更新後の指示を使えるようにします。

> [!CAUTION]
> 生成されたコードが設定した標準から外れることがあります。Copilot の動作は確率的です。この演習では、指示を更新した後の傾向の変化を確認します。出力が一字一句一致することを期待しないでください。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発のガイドラインと要件があります。

- データ レイヤーには常に単体テストが必要です。
- UI はダーク モードで、現代的なデザインにします。
- コードには TSDoc ドキュメント コメントを追加します。
- ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件を Copilot が理解しやすいよう、コンテキストや好みを伝えられます。コーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定のファイルの種類に適用する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリへの**すべて**のリクエストで Copilot に送信される単一の指示ファイルです。Copilot Chat や CLI へのリクエストの多くに関連するプロジェクト全体の情報を記述します。技術スタック、構築するものの概要、ベスト プラクティス、全体に適用するガイダンスなどが含まれます。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイル形式の指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントや新しい単体テストの作成などのタスク向けにガイドラインを指定できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示ファイルと `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルにも対応しています。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイル作成の全体を説明することはこのワークショップの範囲外ですが、サンプル プロジェクトの例は一般的な方法を示しています。

- `copilot-instructions.md` には、構築するもの、プロジェクトの構造、全体に適用するコーディング標準など、プロジェクト レベルのガイダンスを記述します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ レイヤーなどのファイル形式やタスクに応じた指示を記述します。
- 自然な言葉で明確なガイダンスを書き、コードのよい例と悪い例を示します。

指示ファイルを作成する方法は 1 つではありません。AI を使うほかの作業と同様に、試しながらプロジェクトに適した方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示があります。
>
> Copilot に指示ファイルを作成してもらうこともできます。各環境で手順は異なります (VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など)。該当する環境のレッスンで説明します。
>
> テンプレートや出発点を探すには、指示ファイル、カスタム エージェントなどを集めたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、基本の `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルが含まれています。エディターまたは GitHub Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスにも注目してください。Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ レイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob は (リポジトリのルートを基準に) 指示の適用先を指定します。ここでは、`**/*.test.ts` などに一致するすべての TypeScript テスト ファイルに適用されます。
5. このプロジェクトの単体テスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。このように指示を小さな再利用可能なファイルに分け、コード生成時に参考にする例を示せます (パスは指示ファイルを基準にします)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内のドキュメントはまだ要件になっていません。次の手順で、TSDoc コメントとファイル ヘッダー コメントのルールを追加します。

## 変更用のブランチを作成する

ベスト プラクティスに沿って変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドで新しいブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する**前に** Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルは演習によって異なるため、必要な手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新した後に同じプロンプトを再送信して、違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを使えるようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置先を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択して、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   `src/lib/publishers.ts` を作成または更新し、すべての発行元について名前と ID の一覧を返すデータ アクセス ヘルパーを追加してください。ファイルを変更してください。
   ```

5. Copilot がプロジェクトを調査し、ヘルパー ファイルやテストなどのコードを更新します。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取り、型付きの発行元配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約によるものです。
7. 提案されたコードに TSDoc ドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメント コメントを追加する可能性があります。その場合も問題ありません。この演習の目的は、指示の更新によって一貫性が改善する傾向を確認することです。

## リポジトリの新しい標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コードの提案を改善するため、コーディング標準を追加します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コード標準は記載されていますが、コード内ドキュメントのルールがありません。そのため、生成されたヘルパーにもコメントがありませんでした。
3. 既存の標準のすぐ下に、次の内容を追加して、ファイル ヘッダー コメントと TSDoc コメントを Copilot に求めます。

   ```markdown
   - すべてのエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc コメントを記述します。
   - import 文やコードの前に、ファイルの役割を説明するコメント ブロックを追加します。
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示は `.github/copilot-instructions.md` にリポジトリ全体のルールとして、または言語、ファイル形式、タスク別の `*.instructions.md` として作成できます。ここで追加したドキュメント コメントのルールのようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   `src/lib/publishers.ts` を作成または更新し、すべての発行元について名前と ID の一覧を返すデータ アクセス ヘルパーを追加してください。ファイルを変更してください。
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されたことを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディング プラットフォームの発行元データ アクセス ヘルパーです。
    * データベースから発行元情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * すべての発行元について ID と名前の一覧を返します。
    *
    * @param db - Drizzle データベース クライアント。
    * @returns 発行元オブジェクトの配列に解決される Promise。
    */
   ```

Copilot を導いて、新しいプロジェクト標準を次の演習で使用する実際のコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもリポジトリ内のほかのファイルと同様に、ソース管理で管理します。変更をコミットしてブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. 次のコマンドを実行し、指示とヘルパーの変更が存在することを確認します。

   ```bash
   git status
   ```

3. 指示ファイルの更新と、ヘルパーの基盤として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 次のコマンドでブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたので、バックログにあるドキュメント標準の Issue に関連付けたプルリクエストを作成します。手動でも作成できますが、接続済みの GitHub ツールを使って Copilot に依頼しましょう。Issue を検索し、Issue をクローズする PR を作成して、マージします。

> [!NOTE]
> この演習では初めて Copilot に GitHub 上の操作を依頼します。プロジェクトのテンプレートには GitHub ツールがすでに設定されているため、追加のセットアップは不要です。Copilot が初めてツールを使用するとき、VS Code で GitHub へのサインインが必要な場合があります。プロンプトに従って許可してください。後の演習で、この接続に使われる Model Context Protocol を説明します。

1. codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を検索し、現在のブランチから PR を作成するよう、次のプロンプトで Copilot に依頼します。指示の更新と発行元ヘルパーの基盤の両方を説明に含めます。

   ```plaintext
   指示ファイルの更新に関連する Issue を見つけてください。現在のブランチから新しい PR を作成し、その Issue をクローズすることを明記してください。また、今後のフィルター作業に備えて発行元ヘルパーの基盤も追加したことを説明に含めてください。
   ```

4. Copilot が Issue を検索して PR を作成します。
5. Copilot が GitHub の操作を行うことを **Allow** するよう求められたら、コマンドを確認して適切に許可します。
6. PR が作成されたら、次のプロンプトでマージし、ブランチをローカルの `main` に戻すよう依頼します。

   ```plaintext
   PR を `main` にマージしてください。その後、ローカルで `main` に戻り、最新のコードを pull して作業ツリーを更新してください。
   ```

7. Copilot に GitHub 操作やシェル コマンドを許可するよう求められたら、内容を確認して適切に許可します。

GitHub Copilot の支援を受けてプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを読み込む方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- *既存の*指示ファイルでコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再送信し、提案されたコードに新しい標準が反映される様子を確認しました。
- 指示ファイルの更新とヘルパーの基盤を `main` にコミットし、次の演習で使えるようにしました。

次は[エージェント モード][next-lesson]で指示を活用し、コードベース全体にわたる新機能を追加します。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [よりよいカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
