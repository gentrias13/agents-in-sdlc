---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行できます。エージェントは作業を終えると、人間がレビューできるドラフト PR を作成します。面倒なタスクを委任して開発を加速できるため、開発者はより大きな課題に集中できます。

このレッスンでは、Copilot cloud agent を使って次のことを行います。

- コードを生成する環境をカスタマイズする
- 操作が安全に実行されるようにする
- 範囲を明確に定めた Issue の重要性を理解する
- Issue を Copilot に割り当てる

## シナリオ

Tailspin Toys には対応したい技術的負債があります。最初にサイトを作成した請負業者がドキュメントを残さなかったため、まずはアプリケーションのエクスポート関数すべてに TSDoc ドキュメント コメントを追加したいと考えています。

また、デザイン チームはゲームを見つけやすくしたいと考えています。ゲームの詳細ページに同じカテゴリのタイトルを「関連ゲーム」として表示し、ユーザーが閲覧を続けられるようにします。まだ完成度の高い実装は必要ありませんが、UX の受け入れテストに使えるものを求めています。現在はブロッカーですが、ほかの Issue の優先度が高いため後回しになっています。

どちらも優先度が下がりやすいタスクの例で、Copilot cloud agent に割り当てるのに適しています。Copilot cloud agent は非同期に作業するため、開発者はほかのタスクに取り組み、後で戻って提案内容をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]など、複数の方法で作業を依頼できます。割り当てると、Copilot は進捗を追跡するドラフト プルリクエストを作成し、環境をセットアップしてタスクに取り組みます。作業中または完了後に Copilot のセッションを確認できます。提案された解決策をレビューできる状態になると、プルリクエストで通知されます。

## 範囲を明確に定めた指示の重要性

GitHub Copilot に魔法はありません。考える作業を省けるわけではありません。一見簡単な操作でも、詳しく見ると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲を明確に定めてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階的に作業し、学び、試して、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と、それが Copilot を導く方法を確認しました。カスタム指示の演習を完了した場合は `.github/copilot-instructions.md` の動作を確認済みです。そうでない場合も、この機会に一度確認しましょう。Copilot cloud agent に作業を割り当てる前に、リポジトリに含まれる指示ファイルを読み取り専用で確認し、後でその効果を確認できるようにします。

GitHub Web UI、または起動済みの場合は codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメント コメントに関するルールを確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` のファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てるとき、生成されたプルリクエストにこれらの指示による TSDoc コメント、TypeScript の規約、ファイル ヘッダーが反映されているかを確認します。後の演習で PR をレビューするときにも、改めて確認します。

## Copilot cloud agent の開発環境をセットアップする

コードを作成するには、誰が作業する場合でも環境の準備が必要です。タスクを Copilot に割り当てる場合も、ソフトウェア エンジニアと同様のセットアップが必要です。

Cloud agent は、作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に構成する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業を始める前に必要な開発ツールや依存関係を用意できます。このラボでは学習をスムーズに進められるよう、すでに構成されています。Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、単一の Astro アプリ用にマイグレーションとシードを済ませた SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *各手順*に必要な最低限のアクセス許可を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体で Astro + Drizzle/libSQL を使用します)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを利用できるよう、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは通常の GitHub ワークフローと似ていますが、重要な点があります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot がプルリクエストの作業を開始する前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーがあります。リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot の作業を待たずにテストできるため便利です。

## ドキュメントを追加する

ドキュメントの重要性は広く知られていますが、情報が古かったり、ドキュメントが不足していたりするプロジェクトが多くあります。こうした技術的負債が放置されると、生産性が低下し、コードベースの保守や新しいメンバーの参加が難しくなります。Copilot はドキュメント作成を得意とするため、cloud agent に依頼するのに適しています。バックグラウンドで必要なドキュメントを生成し、後の演習で結果を確認します。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `コードにドキュメントが不足している` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   必要に応じて、関数とメソッドに TSDoc ドキュメント コメントを含めることが組織の要件です。最近の更新はこの標準に従っていません。動作の説明に役立つ箇所にドキュメント コメントを追加するよう、既存のコードを更新してください。
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。PR が一覧にない場合は、少し待ってからブラウザーを更新します。
12. 数分後に Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかることがあります。Copilot は環境を作成し (前に確認したワークフローを実行します)、プロジェクトを分析して、問題に取り組む方法を決めます。

13. todo リストを確認し、Copilot が実行するタスクを読みます。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッション ビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを確認するために、ウィンドウの更新が必要な場合があります。

16. ライブ セッションをスクロールして、Copilot がコードを調査して状態を理解し、計画を検討して判断し、コードを作成する様子を確認します。

この作業には数分かかることがあります。Copilot cloud agent の主な目的は、ほかの作業を続けている間にタスクを非同期で実行することです。別のタスクを Copilot cloud agent に割り当ててから、アプリケーションに機能を追加するコードを作成してみましょう。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは作業を分担できることです。デザイン チームが求める関連ゲームの追加に時間がかかるかどうかに関係なく、その時間をほかの作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `ゲーム詳細ページに関連ゲームを表示する` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   ゲーム詳細ページに関連ゲームを表示して、ユーザーがより多くのゲームを見つけられるようにしたいと考えています。デザイン チームは UX を検討し、受け入れテストを行う予定です。要件は次のとおりです。

   - `src/lib/` にデータ アクセス ヘルパーを追加し、指定したゲームを除く、同じカテゴリのほかのゲームを返す
   - ゲーム詳細ページに、このヘルパーを使う「関連ゲーム」セクションを表示する
   - 関連ゲームがない場合に対応する
   - 新しいヘルパーの単体テストを作成する
   - PR を作成する前に、すべてのテストが成功することを確認する
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることを示します。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同様に作業するため、常に監視する必要はありません。完了後に結果をレビューできます。次はカスタム エージェントの作成と使用に進みます。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に Issue を割り当て、非同期で作業を依頼できます。技術的負債の解消、新機能の作成、コードの別フレームワークへの移行にも利用できます。

この演習では、次のことを行いました。

- コードを生成する環境をカスタマイズする
- 操作が安全に実行されるようにする
- 範囲を明確に定めた Issue の重要性を理解する
- Issue を Copilot に割り当てる

Cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントを作成して使用します。[Copilot cloud agent では MCP サーバー][cloud-agent-mcp]も使えます。また、前のモジュールで確認したカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
