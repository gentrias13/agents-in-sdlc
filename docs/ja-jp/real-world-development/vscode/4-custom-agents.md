---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、集中的な指示、ベスト プラクティス、コーディング パターン、分野固有の知識を与えることで、特定の作業をより効果的に実行できます。たとえば、[WCAG][wcag] 準拠を徹底するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントなど、チームの専門知識を再利用可能なエージェントとしてまとめられます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` に置いた Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を定義する Markdown プロンプトを記述します。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には論理的な重なりがあります。どちらも主に Markdown ファイルで定義し、作業の実行方法を AI に伝えます。最も簡単な区別は、**カスタム エージェントは作業者、スキルはツール**ということです。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業を行うためにスキル (場合によってはほかのエージェント) を連携させます。このラボでは、アクセシビリティ カスタム エージェントがガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエスト ワークフローやテストを実行・管理するスキルを呼び出すことができます。

> [!NOTE]
> カスタム エージェントを作成する正解は 1 つではありません。AI を使うほかの作業と同様、環境やシナリオに合わせて試行し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、見え方や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるように取り組んでいます。最近のフィードバックでは、テキストと背景のコントラストが不足しているため、現在のダーク テーマが読みにくいという意見がありました。デザイン チームは、ユーザーがオンとオフを切り替えられるハイコントラスト モードを追加したいと考えています。

アクセシビリティは重要なので、機能をすばやく実装する必要があります。カスタム エージェントを使って機能を生成しましょう。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイコントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。Copilot をどのように導くか確認しましょう。

Codespace に戻り、ターミナルを開いて、アクセシビリティ作業用に `main` から新しいブランチを作成して切り替えます (演習 3 のフィルター PR と分けます)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む frontmatter が必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシビリティ対応の Web サイトを生成する際の主な責任
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント ドロップダウンには、`.github/agents` に定義されたカスタム エージェントが表示されます。エージェントを選択すると、チャット セッションにそのエージェントの指示とツールが適用されます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルは演習によって異なるため、必要な手順で説明します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します (まだ選択されていない場合)。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

2. チャット ビュー下部のエージェント ドロップダウン (アクティブなエージェントを表示します。既定は **default**) を選択します。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

   ```plaintext
   サイトにハイコントラスト モードを追加してください。ユーザーがハイコントラストを切り替えられるようにし、設定をブラウザーの local storage に保存してページの再読み込み後も維持してください。
   ```

5. Copilot Chat がコードベースを調査し、変更を提案してプロジェクトのファイルを編集します。各編集は、ファイル パスとレビュー可能な差分とともにチャット内に表示されます。
6. 編集が適用されると **Files changed** の表示が更新され、エージェントが変更した作業ファイルを確認できます。

> [!NOTE]
> この処理には数分かかることがあります。Copilot はリポジトリのファイルを実際に変更します。Astro コンポーネント、CSS、関連テストなどを編集する場合があります。

次の演習では、作業中のセッションを確認して指示を追加します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。カスタム エージェントは、特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの専門知識や標準を再利用可能なエージェントにまとめ、より効果的な作業を支援できます。

この演習では、次のことを学びました。

- カスタム エージェントの定義方法
- Copilot Chat のエージェント モードでカスタム エージェントを使う方法

次は[エージェントの作業を監視して指示を出し][next-lesson]、変更を確認しながら同じセッションでライト モードの切り替え機能を追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
