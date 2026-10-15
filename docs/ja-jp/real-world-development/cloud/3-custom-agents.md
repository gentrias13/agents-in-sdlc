---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、集中的な指示、ベスト プラクティス、コーディング パターン、分野固有の知識を与えることで、特定の作業をより効果的に実行できます。たとえば、[WCAG][wcag] 準拠を徹底するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントなど、チームの専門知識を再利用可能なエージェントとしてまとめられます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` に置いた Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を定義する Markdown プロンプトを記述します。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には論理的な重なりがあります。どちらも主に Markdown ファイルで定義し、作業の実行方法を AI に伝えます。最も簡単な区別は、**カスタム エージェントは作業者、スキルはツール**ということです。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業を行うためにスキル (場合によってはほかのエージェント) を連携させます。このラボでは、アクセシビリティ カスタム エージェントがガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエスト ワークフローやテストを実行・管理するスキルなどを呼び出すことができます。

> [!NOTE]
> カスタム エージェントを作成する正解は 1 つではありません。AI を使うほかの作業と同様、環境やシナリオに合わせて試行し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタム エージェントを使って、次のことを学びます。

- カスタム エージェントの定義方法
- カスタム エージェントへのタスクの割り当て方法

## シナリオ

Tailspin Toys は、見え方や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるように取り組んでいます。最近のフィードバックでは、テキストと背景のコントラストが不足しているため、現在のダーク テーマが読みにくいという意見がありました。デザイン チームは、ユーザーがオンとオフを切り替えられるハイコントラスト モードを追加したいと考えています。

アクセシビリティは重要なので、機能をすばやく実装する必要があります。カスタム エージェントを使って機能を生成しましょう。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。Copilot をどのように導くか確認しましょう。

Codespace に戻り、アクセシビリティ カスタム エージェントのファイルを開きます。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む frontmatter が必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシビリティ対応の Web サイトを生成する際の主な責任
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## Issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを管理する場所です。Copilot cloud agent にタスクを割り当て、進行状況を監視し、指示を追加できます。まず、ハイコントラスト モードの作成を Copilot に割り当てます。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Web サイトにハイコントラスト モードを追加する` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   サイトにハイコントラスト モードが必要です。ユーザーがハイコントラストを切り替えられるようにしてください。設定はブラウザーの local storage に保存してください。
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

   ![カスタム エージェントとして Accessibility を選択した cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。カスタム エージェントは、特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの専門知識や標準を再利用可能なエージェントにまとめ、より効果的な作業を支援できます。

この演習では、次のことを学びました。

- カスタム エージェントの定義方法
- カスタム エージェントへのタスクの割り当て方法

Copilot がハイコントラスト モードの実装に取り組んでいる間に、次は mission control から[エージェント セッションを監視して指示を出します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使うための準備][org-custom-agents]
- [Enterprise でカスタム エージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
