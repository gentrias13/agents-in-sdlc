---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローに取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、フィルター用の Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果をレビューし、最初のプルリクエスト (PR) を手動でマージしました。
- フィルターの Issue から始め、Plan モードでアプローチを決め、Autopilot モードで構築し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルターを確認しました。
- 品質保証 (QA) のカスタム エージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの証拠を確認しました。
- フィルターの変更全体をレビューし、Agent Merge にフィルター PR のマージを許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュ コマンドを確認しました。

## リリースした内容

このワークショップには、更新した `main` からそれぞれブランチを作る、2 つの PR のマイルストーンがあります。

1. **星評価**: 既存の `starRating` と、評価がない場合の表示をゲームカードに追加します。
2. **フィルターと品質ワークフロー**: フィルターを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズして QA プロファイルを作成し、関連するテストを追加します。

フィルターを計画してから PR を作成するまで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業を 1 つの PR にまとめました。

## 検証方法

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー操作など、複数の方法でコードを検証しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で結果を報告します。QA は要件とテスト範囲のレビューにこれらの結果をまとめ、PR の前に確認します。

追加テストは実際の不足を補うものである必要があります。追加テストが不要という QA の結果が正しい場合もあります。マージを許可する前にコードと証拠を確認し、変更を加えた後は関連する検証結果を更新します。

## ベスト プラクティス

Copilot に渡すコンテキストとツールによって、作業結果が変わります。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し可能なタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。構築前にアプローチを考えるには **Plan**、範囲の絞られた変更に取り組みながら確認を続けるには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選択します。

基盤と同様に、コンテキストも重要です。*何を*、*なぜ*、*どのように*構築するのかを明確に説明すると、結果が大きく変わります。

## さらに試す

基本的なワークフローを学びました。Copilot CLI には、次のような機能もあります。

- `/review` でコードレビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を整理し、別の視点から意見を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別タスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送信します。

## 次のステップ

ツールを上達させるには、使い続けるのが一番です。本番コードや趣味のコード、長い間作ろうと思っていたアプリで試してください。学んだことをチームで共有し、ほかのメンバーからも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進んでください。ほかの環境での利用方法を比べるには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
