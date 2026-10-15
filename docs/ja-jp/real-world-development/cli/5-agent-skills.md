---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使用する"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルターの検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、必要な作業があります。手動でコードを検証し、指示ファイルを使って標準に従うようにしました。テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

こうしたタスクには**エージェント スキル**が適しています。スキルを使うと、操作を正しく実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使います。結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルには、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめられます。基本的にはスキル名のフォルダーにある `SKILL.md` で構成されます。この Markdown ファイルには、スキル名と説明を定義する frontmatter、スキルの概要、スキルを呼び出すタイミングのガイダンスを記述します。フォルダーにスクリプトやそのほかのリソース用サブフォルダーを含めることもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトに既存のコマンドを使うため、`SKILL.md` のみで構成されます。

スキルをプロジェクトの `.github/skills` フォルダーに置くと、チームで共有して再利用できます。または、ユーザーのスキル フォルダー `~/.copilot/skills` に置くこともできます。

## スキルを確認する

Tailspin Toys のチームが単体テスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 冒頭の `name` と `description` を読みます。説明は Copilot がスキルを呼び出すタイミングを理解するために使われます。
3. 指示を読み、テストと lint のプロセスを Copilot に案内する方法を確認します。
4. まだ **結果の出力形式** セクションがないことを確認します。

## 変更を加える前にスキルを実行する

Copilot CLI から直接、または自然言語でスキルを呼び出せます。3 つのチェックを実行するよう Copilot に依頼しましょう。

1. Interactive モードでフィルターの会話に戻ります。
2. 次のプロンプトを入力します。

   ```plaintext
   `quality-checks` スキルを実行して、単体テスト、lint、型チェックを行ってください。
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したかどうか、ツールが報告した内容を確認できるレポートに改善しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## 結果の出力形式

   完了したら、実行した各コマンドと、成功、失敗、またはブロックされたかどうかを報告してください。テスト件数、所要時間、エラー、警告、そのほかの指標は、ツールが報告した場合にのみ含めてください。失敗やブロッカーに対する次の対応を示し、スキップしたチェックや未完了のチェックを成功と報告しないでください。
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

Copilot CLI は会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどと同じプロンプトを入力します。

   ```plaintext
   `quality-checks` スキルを実行して、単体テスト、lint、型チェックを行ってください。
   ```

3. 最後のレポートを確認し、最初の結果と比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使用しました。次のことを行いました。

- 単体テスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更はフィルターの機能 PR に含まれます。次は、[Playwright MCP サーバーを使って Copilot がサイトを直接操作できるようにします][next-lesson]。

## スキルの例

以下のコミュニティの例は参考資料であり、追加の作業ではありません。

- [Agent Skills の仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とバンドルされたエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
