---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを進める必要がある場合や、Copilot が知っておくべき背景情報がある場合は、コンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に希望する作業だけでなく、その進め方も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code と CLI の harness とは異なり、ここではプロンプトの「変更前と変更後」を比較しません。Copilot cloud agent は GitHub Issue に対して非同期で作業するため、リアルタイムの比較が難しいためです。この harness の後半で、指示ファイルが cloud agent のプルリクエストにどう反映されたかを確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- データ レイヤーには常に単体テストが必要です。
- UI はダーク モードで、現代的なデザインにします。
- コードには TSDoc ドキュメント コメントを追加します。
- ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件を Copilot が理解しやすいよう、コンテキストや好みを伝えられます。好みのコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定のファイルの種類に適用する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリへの**すべて**のリクエストで Copilot に送信される単一の指示ファイルです。Copilot へのチャットや CLI のリクエストの多くに関連するプロジェクト全体の情報 (使用する技術スタック、構築するものの概要、ベスト プラクティス、全体に適用するガイダンスなど) を記述します。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイル形式の指示ファイルです。TypeScript や Astro などの特定の言語や、UI コンポーネントや単体テストの作成などのタスク向けにガイドラインを指定できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示ファイルと `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> Copilot は、AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルにも対応しています。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイル作成の全体を説明することはこのワークショップの範囲外ですが、サンプル プロジェクトの例は一般的な方法を示しています。

- `copilot-instructions.md` には、構築するもの、プロジェクトの構造、全体に適用するコーディング標準など、プロジェクト レベルのガイダンスを記述します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ レイヤーなどのファイル形式やタスクに応じた指示を記述します。
- 自然な言葉で明確なガイダンスを書き、コードのよい例と悪い例を示します。

指示ファイルを作成する方法は 1 つではありません。AI の使い方と同様に、試しながらプロジェクトに適した方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを作成してもらうこともできます。各環境で手順は異なります (VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など)。該当する環境のレッスンで説明します。
>
> テンプレートや出発点を探すには、指示ファイル、カスタム エージェントなどを集めたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、基本の `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルが含まれています。エディターまたは GitHub Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスにも注目してください。Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ レイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob は (リポジトリのルートを基準に) 指示の適用先を指定します。ここでは、`**/*.test.ts` などに一致するすべての TypeScript テスト ファイルに適用されます。
5. このプロジェクトの単体テスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などのほかの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。このように指示を小さな再利用可能なファイルに分け、コード生成時に参考にする例を示せます (パスはリポジトリのルートではなく指示ファイルを基準にします)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内のドキュメントはまだ要件になっていません。次の手順で、TSDoc コメントとファイル ヘッダー コメントのルールを追加します。

## リポジトリの新しい標準を追加する

ここで編集するのは 1 か所です。コード内のドキュメントとして TSDoc コメントとファイル ヘッダーを追加するよう、プロジェクト全体のルールを設定します。後の演習で Issue を割り当てると、cloud agent がこのルールを使用します。

編集前にブランチを作成します (cloud agent は Issue の対象ブランチから指示を読み込みますが、変更を明確にコミットするためにブランチを使います)。

1. Codespace のターミナルで、次のコマンドを実行して新しいブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

先ほど確認したように、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コードにドキュメントを追加するルールを設定して、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コーディング標準が記載されていますが、コード内のドキュメントに関するルールはまだありません。
3. 既存の標準のすぐ下に、ファイル ヘッダーと TSDoc コメントを追加する次の内容を挿入します。

   ```markdown
   - すべてのエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc コメントを記述します。
   - import 文やコードの前に、ファイルの役割を説明するコメント ブロックを追加します。
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示は `.github/copilot-instructions.md` にリポジトリ全体のルールとして、または言語、ファイル形式、タスク別の `*.instructions.md` として作成できます。ここで追加したドキュメント コメントのルールのようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

Cloud agent は Issue の対象ブランチから指示ファイルを読み込みます。次の演習では Issue を `main` に対して Copilot に割り当てるため、指示を `main` にマージしておく必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で Issue を割り当てると、cloud agent はこの指示を使います。

> [!TIP]
> ワークショップでは `main` に直接作業することもできます。その場合は、ブランチを作る手順を省略して `main` にコミットします。このブランチを使う手順は、実際のプロジェクトでの進め方を反映しています。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを読み込む方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。残りの harness では、この標準が cloud agent のプルリクエストに反映されているかを確認します。

次は、[Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのよりよいカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
