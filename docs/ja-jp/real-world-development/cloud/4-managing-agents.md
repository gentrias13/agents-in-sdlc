---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

直前の 2 つの演習では、ユーザー エクスペリエンスの改善と機能追加に関する 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

GitHub.com の[agents ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールが用意されています。この mission control では、開いているプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行された操作を調べ、進行中のセッションに指示を出すこともできます。

このレッスンでは、次のことを行います。

- agents ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイコントラスト モードの作成をエージェントに依頼した後、チームはライト モードも追加するよい機会だと考えました。サイトのスタイル更新と切り替え機能をすでに実装しているため、ライト モードも含めるのが自然です。エージェントに指示を追加して、ハイコントラストとライト モードの両方を実装するよう依頼します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインの両方にあるタスクの一覧を確認します。次のように Copilot に割り当てたタスクが表示されます。
   - コードのドキュメントを更新する
   - ゲームを変更する API を生成する
   - Web サイトにハイコントラスト モードを追加する
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。次のような作業が含まれることがあります。
   - リポジトリからコードをチェックアウトする
   - Copilot の環境を作成する
   - MCP サーバーを設定する
   - タスク完了に必要な操作を実行する

> [!NOTE]
> 表示される手順は、作業の状態と Copilot が選んだアプローチによって異なります。

4. 右側に表示される PR ペインも確認します。ここから PR と変更されたファイルを監視できます。

## Cloud agent に追加の指示を出す

実行中のタスクを確認したので、ハイコントラスト モードに取り組んでいる Copilot にライト モードも追加するよう依頼します。

1. ハイコントラスト モードの追加を示すセッションを選択します。正確なタイトルは、Copilot が付けた名前と作業の状態によって異なります。

   ![Mission control に表示された Accessibility セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業を始めたことを示すメッセージが表示されるまで、数分間セッションを監視します。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを追加します。

   ```plaintext
   ハイコントラスト モードに加えて、ライト モードも追加してください。ユーザーが希望する表示モードを選択できるよう、ライト モード用のスイッチも追加してください。
   ```

   ![Copilot が作業中のセッションに指示を追加するダイアログが強調表示された agents ページ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業計画に組み込むことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組み始めます。メッセージを送信したときに処理していた手順を終えると、新しい依頼をワークフローに追加します。

今回も完了まで数分かかります。その間に、これまでに学び確認した内容を振り返ってください。

## まとめと次のステップ

GitHub Copilot cloud agent のタスクを監視して指示を出すための中心的な場所である Copilot agents ページについて学びました。この mission control を使うと、実行中と完了済みのタスクを追跡し、実行内容を確認して、進行中のタスクの範囲を変更したり追加のガイダンスを提供したりできます。

このレッスンでは、次のことを学びました。

- mission control と agents ページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
