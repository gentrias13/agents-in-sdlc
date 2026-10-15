---
title: "演習 5: GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善や機能追加を目的として、GitHub Copilot に複数のタスクを依頼しました。コードへのドキュメント追加、デザイン チーム向けの関連ゲーム機能の作成、ハイコントラストとライト モードの切り替えによるアクセシビリティ対応を行いました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を導入してもソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進めます。チームのほかのメンバーにレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認しましょう。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期かつ無人でタスクを実行するため、安全性を確保するための制限があります。

- Copilot はリポジトリの読み取りアクセス権を持ち、書き込みアクセスはコード変更に使用するブランチのみに制限されます。
- Cloud agent は GitHub Actions で実行され、作業用に独立した一時的な環境を作成します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- 既定では、[外部リソースへのアクセス][agent-firewall]が制限されています (MCP サーバーを含みます)。

## 生成されたドキュメントをレビューする

GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するときに、ファイアウォールによって GitHub Copilot がブロックされたという警告が表示されることがあります。Copilot は既定で外部リソース (外部 MCP サーバーへの呼び出しなど) へのアクセスが制限されているため、これは**想定された動作**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択してプルリクエストの一覧を開きます。
3. **ドキュメントの不足を補う** などのタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだタスクを実行中の場合、PR のタイトルに **[WIP]** が含まれています。その場合は作業が完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら、**Files changed** タブを選択して変更を確認します。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc ドキュメント コメントなど、新しく追加されたドキュメントを確認します。具体的な変更は異なる場合があります。

   TSDoc コメント、TypeScript の規約、ファイル ヘッダーがあるかを確認してください。これらは演習 2 の最初に確認したカスタム指示ファイルの内容です。

6. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示すインジケーターを確認します。
8. 承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローがキュー登録されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と共同作業できます。チーム メンバーと同じように、PR のコメントやコードへのインライン コメントで Copilot にメンションすると、コメントを読み取って対応するセッションを開始します。結果は確定的ではないため、依頼する内容を厳密に指定することはできません。たとえば、次の変更を依頼できます。

- 各コード ファイルの先頭に、ファイルの役割を説明する短いコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc ドキュメント コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例などを参考に、コードのドキュメントを更新するコメントを追加し、ユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot が変更を加える様子を確認します。Copilot は更新のために新しいセッションを開始します。
3. **Back to pull request** を選択すると、PR に戻れます。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了したら、プルリクエストに新しいコミットが追加されていることを確認します。
5. **Files changed** タブを選択して変更をレビューします。

満足するまで反復作業を続けます。問題がなければ、PR をドラフトからレビュー可能に変更して `main` にマージできます。

![PR をレビュー可能に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **ゲーム詳細ページに関連ゲームを表示する** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちになっていることを示すインジケーターを確認します。
7. 承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローがキュー登録されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
9. **オプション:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。ほかの cloud agent の作業と同様に、コメントを追加して変更を依頼できます。たとえば、表示する関連ゲームの数を変えたり、**カスタム指示の更新前**に依頼したタスクなので、コメント ヘッダーや TSDoc コメントを追加したりできます。**Conversation** タブでコメントすると、Copilot が内容を確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントが実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラスト モードと、演習 4 で mission control から依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Web サイトにハイコントラスト モードを追加する** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだタスクを実行中の場合、PR のタイトルに **[WIP]** が含まれています。その場合は完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認します。特に次の点に注目してください。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を保存するための local storage の使用
   - ハイコントラスト モードとライト モード用の CSS またはスタイルの変更
   - ARIA ラベルやキーボード操作など、アクセシビリティ属性
   - モードの切り替えを管理する JavaScript/TypeScript コード

6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示すインジケーターを確認します。
8. 承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローがキュー登録されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
10. **オプション:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   アプリケーションを起動し、ハイコントラストとライト モードの切り替えが期待どおりに機能し、ページの再読み込み後も設定が維持されることを確認します。

アクセシビリティ カスタム エージェントが、アクセシビリティのベスト プラクティスに沿った実装を Copilot に促したことを確認してください。アクセシビリティ上の懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** をメンションして更新を依頼できます。

## オプションの演習 — 委任を続ける

Cloud agent に実際のバックログの項目を任せ、その間に別の作業を進める方法を練習しましょう。リポジトリに Issue を追加で作成し、Copilot に割り当てます。次のようなタスクを試せます。

- ゲーム詳細ページに支援者の関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力の検証とエラー処理を追加する。

## まとめ

Cloud agent harness を完了しました。各レッスンで次のことを行いました。

- **このリポジトリのカスタム指示ファイルを確認**し、cloud agent の出力に反映される内容を把握しました。
- **Issue を Copilot cloud agent に割り当て**、環境を準備し、非同期で計画と実行を行う様子を確認しました。
- **アクセシビリティ用のカスタム エージェントを作成して使用**し、ハイコントラストとライト モードの切り替えを追加しました。
- **Copilot agents ページを mission control として使用**し、セッションの作業を監視して指示を出しました。
- **cloud agent のプルリクエストをレビューして反復**し、`@copilot` をメンションして変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent harness を完了しました。さらに学ぶには、次の harness も活用できます。

- 🖥️ **[VS Code harness](../../vscode/)** — IDE で Copilot Chat のエージェント モードと MCP 統合を試します。
- 💻 **[CLI harness](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に接続するオプションの `/delegate` を使い、ターミナルから 2 つの変更をレビューしてリリースします。

自分のリポジトリで、次の案を試してみてください。

- リファクタリングまたはテスト カバレッジに関する Issue を cloud agent に割り当てます。
- 別の分野向けのカスタム エージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
