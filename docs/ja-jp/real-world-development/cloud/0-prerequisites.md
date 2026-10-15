---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な準備を整えます。Tailspin Toys リポジトリの自分用コピーを作成し、[codespace][codespaces] を起動します。codespace は、指示ファイルの編集や cloud agent が生成した作業の確認に使います。

## ラボ用リポジトリを設定する

作成するコード用にリポジトリのコピーを用意するには、[テンプレート][template-repository]からインスタンスを作成します。新しいリポジトリにはラボに必要なすべてのファイルが含まれ、演習全体を通して使用します。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボ リポジトリのページで **Use this template** を選択し、**Create a new repository** を選択して自分用のコピーを作成します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップを進めている場合は、メンターの案内に従ってください。それ以外の場合は、GitHub Copilot を利用できる organization に新しいリポジトリを作成できます。

   ![リポジトリ テンプレートの設定を入力する](../../../_images/ex0-repository-settings.png)

4. 後でラボで参照するため、作成したリポジトリ パス (**organization-or-user-name/repository-name**) を控えておきます。

## Codespace を作成する

次に、codespace を使ってラボの演習を進めます。

[GitHub Codespaces][codespaces] はクラウドベースの開発環境です。ブラウザーから直接コードを作成、実行、デバッグできます。複数のプログラミング言語、拡張機能、ツールに対応したフル機能の IDE を提供します。

1. 新しく作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

   ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、すべてのサービスを手動でインストールするより速く完了します。その間に、次に取り上げる GitHub Copilot のほかの機能を確認できます。

> [!CAUTION]
> 後続の演習で codespace に戻ります。いまのところは、ブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] 内で実行するように構成されています。どちらもスムーズに作業できるよう、必要な前提条件をすべて備えています。ローカルで実行するには、クローンしたリポジトリを VS Code で開き、表示されたら **Reopen in Container** を選択してください。VS Code が codespace と同じ dev container を構築します。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ用リポジトリのコピーを作成し、Copilot cloud agent と並行して作業するために使う codespace の作成を開始しました。

## 次のステップ

次は、cloud agent が従うカスタム指示を追加します。[演習 1 - カスタム指示][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
