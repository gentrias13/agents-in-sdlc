---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードを使ってフィルター機能を構築しました。プルリクエストを作成する前に、ブラウザーで正しく動作することを確認します。自分でアプリを操作する代わりに **Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーを操作させ、フィルター機能を手動でテストします。
- ブランチを公開して、フィルターのプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェント モードはエディターの外にあるツールを使うと、さらに便利になります。Model Context Protocol (MCP) は Copilot が外部のツールやサービスと通信するための標準です。

![エージェント モードとコンテキスト、LLM、ツール (MCP サーバーや VS Code 拡張機能が提供するツールを含む) の連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信する方法を提供します。MCP を使うと、AI エージェントはリソースから最新情報にアクセスし、ツールを使ってユーザーに代わる操作を実行できます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。MCP サーバーは、既存の API や npm パッケージなどのローカル ツールを含む外部サービスとの通信を管理します。各 MCP サーバーは、AI エージェントが利用できる異なるツールとリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI エージェントは新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントは Web ページの移動、フォームへの入力、ボタンの選択などを実行できます。

ほかにもさまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub では、エコシステム内のサーバーを見つけやすくし、貢献を促すために [MCP レジストリ](https://github.com/mcp)を提供しています。

> [!CAUTION]
> セキュリティ面では、MCP サーバーをプロジェクトのほかの依存関係と同様に扱ってください。使用前にソース コードをよく確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する場合は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで使用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが構成されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクト テンプレートに含まれています。そのため、前の演習で Copilot が Issue のバックログを読み取ることができました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供します。機能のテストに利用できます。

1. `.vscode/mcp.json` で `github` と同じ階層に `playwright` エントリを追加し、ファイルを次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` は**ローカル** サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定するとブラウザーを画面に表示せずに実行できます。codespace には表示用のデスクトップがないため、この設定が必要です。

> [!NOTE]
> Tailspin Toys ではエンドツーエンド テストに Playwright を使っているため、ブラウザーは通常すでにインストールされています。後で Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行するよう依頼してから再試行します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot がサーバーのツールを初めて必要とするときに、VS Code がサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** アクションが表示されます。選択してサーバーを起動し、構成を確認します。
2. サーバーを信頼するか確認されたら、構成をレビューし、信頼して起動できるようにします。

手動での起動は任意です。スキップした場合は、次の手順で Copilot がツールを必要としたときに VS Code がサーバーを起動し、信頼するか確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを選択して確認する代わりに、エージェントにページを開いて移動させ、フィルターを適用し、結果を読み取らせて、確認した内容をまとめてもらえます。会話を離れずに機能が期待どおりに動作するか確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。ボタン、リンク、リスト項目など、ラベル付けされた構造化要素を使ってエージェントが判断するため、機能の簡単なテストがアクセシビリティの簡易確認にもなります。

サーバーが接続され、アプリが起動していることを確認したら、次のプロンプトで Copilot にフィルター機能を試すよう依頼します。

```text
Playwright MCP サーバーを使って、実行中のアプリ `http://localhost:4321` をブラウザーで開き、新しいゲーム フィルター機能を検証してください。

1. ゲーム ページに移動し、一覧に表示されたゲームの数を記録します。
2. カテゴリ フィルターを適用し、そのカテゴリのゲームだけが一覧に表示されることを確認します。
3. フィルターをクリアし、発行元フィルターを適用して、その発行元のゲームだけが一覧に表示されることを確認します。
4. カテゴリと発行元のフィルターを組み合わせ、両方が結果に反映されることを確認します。

各手順で確認した内容を報告し、期待どおりに動作しない点があれば指摘してください。
```

Copilot は Playwright MCP サーバーからブラウザーを起動し、各手順を実行して結果を報告します。Issue の受け入れ基準とレポートを照らし合わせます。問題があれば、追加で質問するか、プルリクエストを作成する前にコードの修正を依頼します。

> [!NOTE]
> テストを実行するには、アプリが `http://localhost:4321` で起動している必要があります。開発サーバーを停止した場合は、プロンプトを送信する前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときにブラウザーのダウンロードが必要な場合があります。不足を報告されたら `npx playwright install chromium` を実行してから再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームがレビューできるようプルリクエスト (PR) を作成します。最初に `filtering-vscode` ブランチを公開します。

1. Codespace で **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミット メッセージを生成します。

   ![Source Control パネルに Copilot が行った変更が表示された画面](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) など、PR を作成する方法はいくつかあります。ここでは GitHub Copilot を使って PR を作成します。関連する Issue を見つけて、Issue と関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェント ドロップダウンで **Agent** を選択します。Copilot が GitHub ツールを使えるようにします。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報の収集や操作を実行するときは、必要に応じて **Continue** を選択します。GitHub ツールを初めて使う際は、GitHub での認証を求められる場合があります。画面の指示に従って許可します。
5. Copilot が Issue を検索し、適切なものを見つけて PR を作成します。
6. Copilot が生成したリンクを選択して PR をレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルター機能をテストしてから公開しました。
- ブランチを公開して、フィルターのプルリクエストを作成しました。

MCP サーバーで機能を**テスト**しました。MCP は Copilot を拡張する方法の 1 つです。次は[カスタム エージェントを作成し][next-lesson]、アクセシビリティ レビューのような作業を効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
