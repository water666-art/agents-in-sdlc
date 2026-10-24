---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、作業用の [codespace][codespaces] を起動して、エディターで GitHub Copilot Chat が利用できることを確認します。

## ラボ用リポジトリをセットアップする

コード作業用のリポジトリを作成するには、[テンプレート][template-repository]から新しいインスタンスを作成します。新しいインスタンスにはラボに必要なファイルが含まれており、演習を進める間、このリポジトリを使います。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボ用リポジトリのページで **Use this template** ボタンを選択して、リポジトリのコピーを作成します。次に **Create a new repository** を選択します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントの一環としてワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成できます。

    ![リポジトリテンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えてください。この後のラボで使用します。

> [!NOTE]
> **バックログの準備ができています**
>
> テンプレートからリポジトリを作成すると、GitHub issue のバックログも自動的に作成されます。ワークショップではこれらの issue を使って作業するため、自分で issue を作成する必要はありません。

## codespace を作成する

次に、codespace を使ってラボの演習に取り組みます。

[GitHub Codespaces][codespaces] は、ブラウザーで直接コードを作成、実行、デバッグできるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールに対応した、機能豊富な統合開発環境 (IDE) を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、必要なサービスを手作業でインストールするよりずっと早く済みます。その間に、次の演習で紹介する GitHub Copilot のほかの機能を確認してもよいでしょう。

> [!CAUTION]
> 今後の演習で codespace に戻ります。ここでは、ブラウザーのタブを閉じずに開いたままにしてください。

> [!NOTE]
> このワークショップは、codespace またはローカルの [dev container][dev-containers] 内で実行できます。どちらも、スムーズに作業できるよう必要な前提条件がすべてインストールされた環境を提供します。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、メッセージが表示されたら **Reopen in Container** を選択してください。codespace と同じ dev container が VS Code によってビルドされます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェントモードを使う

GitHub Copilot Chat のエージェントモードを使うには、統合開発環境 (IDE) に GitHub Copilot Chat 拡張機能がインストールされている必要があります。GitHub Codespaces を使っている場合は、通常すでにインストールされています。

> [!TIP]
> GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または、Visual Studio Code の **Extensions** ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

拡張機能をインストールした後、利用するには GitHub アカウントでの認証が必要な場合があります。

1. codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、codespace の上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに「こんにちは」などのメッセージを入力し、<kbd>Enter</kbd> を押します。これで Copilot Chat が起動します。
4. 認証されていない場合は、GitHub アカウントへのサインインを求められます。画面の指示に従って認証してください。

    ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証後、Copilot Chat ウィンドウが表示されます。

## まとめ

ラボ用リポジトリのコピーを作成しました。また、コードを書き始めるときに使う codespace の作成も開始しました。

## 次のステップ

次は Copilot に作業してもらいます。[演習 1 - カスタム指示][next-lesson]に進み、プロジェクトの規約を Copilot に伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
