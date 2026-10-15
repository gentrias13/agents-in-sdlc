---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター、指示、スキルの更新、QA プロファイル、テストをまとめて確認し、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルターの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストが 1 つのブランチに保存されています。まとめて確認し、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は手動でマージしましたが、今回は **Agent Merge** にマージのプロセスを任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔にするため、フィルターと品質ワークフロー全体を 1 つの会話とブランチで進め、1 つの PR にまとめます。

このレッスンでは、次のことを行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びます。
- 機能変更全体と検証結果を確認します。
- フィルターの PR を作成します。
- レビュー後にのみ Agent Merge を有効にして、PR がマージされたことを確認します。

## シナリオ

フィルター機能を計画、実装、検証するために Copilot を使いました。Tailspin Toys では、残りの PR 作業を自動化しつつ、マージの許可は開発者が管理したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージ可能な状態にするために必要な残りの作業を自動化します。有効にすると、Copilot が CI チェックの失敗への対応、レビュー コメントへの返信、必要な場合の rebase など、PR をブロックしている問題を解決します。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは **Merge pull request** を自分で選択していました。Agent Merge にその作業を任せることもできますが、有効にする前に変更をレビューし、問題がないことを確認してください。

## Agent Merge で PR を管理する

コードが完成したので、全体をレビューして PR を作成し、残りの作業を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択して <kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` コマンドで Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功すると、マージが実行されます。
7. Copilot が `PR #14 was squash-merged successfully.` のようなメッセージを表示したことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリの設定、不足しているアクセス許可を回避しません。ブロックされた場合は、報告された理由を確認し、リポジトリで許可されていれば、レビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成とテスト、検証に加えて、プルリクエストのプロセスも自動化しました。次のことを行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びました。
- 機能変更全体と検証結果を確認しました。
- フィルターの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、[GitHub Copilot CLI の便利なスラッシュ コマンド][next-lesson]を確認して、コンテキスト、モデル、共有、任意のクラウド委任について学びます。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
