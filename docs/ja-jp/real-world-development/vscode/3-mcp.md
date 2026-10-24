---
title: "演習 3 - Playwright のモデル コンテキスト プロトコル (MCP) サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェントモードを使ってフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで正しく動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーを操作させ、機能をテストします。その後、ブランチを公開してプルリクエスト (PR) を作成します。

この演習では、次のことを行います。

- モデル コンテキスト プロトコル (MCP) とは何か、MCP サーバーが Copilot に新しいツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーでフィルタリング機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成します。

## モデル コンテキスト プロトコル (MCP) とは

エージェントモードは、エディターの外部にもアクセスできると、さらに強力になります。Copilot がそれを実現する仕組みがモデル コンテキスト プロトコル (MCP) です。MCP は、エージェントが外部のツールやサービスと通信するための標準的な方法です。

![エージェントモードの内部構造と、コンテキスト、大規模言語モデル (LLM)、ツール（MCP サーバーと VS Code 拡張機能が提供するツールを含む）とのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[モデル コンテキスト プロトコル (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、人工知能 (AI) エージェントが外部のツールやサービスと通信できます。MCP を利用する AI エージェントは、外部のツールやサービスとリアルタイムでやり取りできます。これにより、リソースを使って最新情報にアクセスし、ツールを使ってユーザーに代わって操作を実行できます。

これらのツールやリソースには、MCP サーバーを通じてアクセスします。MCP サーバーは、AI エージェントと外部のツールやサービスの間をつなぐブリッジとして機能します。既存のアプリケーション プログラミング インターフェイス (API) や Node Package Manager (NPM) パッケージなどのローカル ツールを含め、AI エージェントと外部ツールとの通信を管理します。各 MCP サーバーは、AI エージェントがアクセスできるツールやリソースのセットを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API セットを利用できます。AI エージェントは、新しいリポジトリの作成、既存リポジトリの更新、issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を利用できます。AI エージェントは、Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

ほかにも、さまざまなツールやリソースを利用できる MCP サーバーがあります。GitHub は、エコシステム内のサーバーを見つけやすくし、貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点では、MCP サーバーもプロジェクト内のほかの依存関係と同じように扱ってください。使用前にソース コードを慎重に確認し、公開元を検証し、セキュリティ上の影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可するときは慎重に判断してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用できる MCP サーバーを構成します。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに構成されていることを確認します。

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

この `github` エントリはプロジェクト テンプレートに含まれているため、Copilot は前の演習で issue のバックログを読み取れました。[リモートの GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot がそのツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを用意できます。これは機能のテストに役立ちます。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、ファイルを次のようにします。

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

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行してサーバーを起動します。`--headless` フラグを指定すると、Playwright は画面にウィンドウを表示せずにブラウザーを実行します。デスクトップ画面がない codespace 内では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトでは、エンドツーエンド テストに Playwright をすでに使っているため、通常は Playwright が必要とするブラウザーもインストールされています。後で Copilot がブラウザーがないと報告した場合は、`npx playwright install chromium` を実行して再試行してください。

## サーバーを起動して信頼する

VS Code は必要なときに MCP サーバーを起動します。Copilot がサーバーのツールを初めて必要としたときに、VS Code がサーバーを起動し、信頼するかどうか確認します。

1. `.vscode/mcp.json` で、`playwright` エントリの上に **Start** アクションが表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、構成を確認してサーバーを信頼し、起動できるようにします。

手動での起動は省略できます。次の手順で Copilot がツールを初めて必要としたときに、VS Code がサーバーを起動し、信頼を求めます。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを開いて操作する代わりに、エージェントにページを開き、移動し、フィルターを適用し、結果を読み取って要約してもらえます。会話を離れずに機能が期待どおり動作することを確認する、手早い方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。エージェントは、ボタン、リンク、リスト項目などの構造化されラベル付けされた要素を、支援技術と同じように扱います。そのため、簡単な機能テストをアクセシビリティの簡易確認にも活用できます。

サーバーを接続し、アプリを実行した状態で、次のプロンプトを使って Copilot にフィルタリング機能を手動テストしてもらいます。

```text
Playwright MCP サーバーを使い、http://localhost:4321 で実行中のアプリをブラウザーで開いて、新しいゲームのフィルタリング機能を確認してください。

1. ゲーム ページを開き、表示されるゲームの数を記録します。
2. カテゴリ フィルターを適用し、そのカテゴリのゲームだけが表示されることを確認します。
3. カテゴリ フィルターを解除し、出版社フィルターを適用して、その出版社のゲームだけが表示されることを確認します。
4. カテゴリと出版社の両方のフィルターを適用し、両方の条件に一致する結果が表示されることを確認します。

各手順で確認した内容を報告し、期待どおりに動作しない点があれば指摘してください。
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、各手順を実行して確認結果を報告します。その内容を issue の受け入れ条件と照らし合わせます。問題がある場合は、追加で質問するか、プルリクエストを開く前にコードを修正するよう依頼してください。

> [!NOTE]
> このテストでは、アプリが `http://localhost:4321` で実行中である必要があります。開発サーバーを停止していた場合は、プロンプトを送る前に再起動してください。Copilot が初めて Playwright MCP サーバーを使うときに、ブラウザーのダウンロードが必要な場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再試行してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が正しく動作することを確認できたので、チームにレビューしてもらうために PR を開きます。最初に `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルを開き、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを選択してコミット メッセージを生成します。

   ![変更内容が表示された Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub のコマンドライン インターフェイス (CLI) などを使ってプルリクエストを作成できます。ここでは GitHub Copilot に作成してもらいます。関連する issue を探し、その issue と関連付けた PR を作成できます。

1. Copilot Chat パネルを開き、**New Chat** を選択して新しいセッションを開始します。
2. エージェントのドロップダウンで **Agent** が選択され、Copilot が GitHub ツールを使えることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   リポジトリでカテゴリと出版社によるフィルタリングに関連する issue を探してください。現在のブランチから新しいプルリクエストを作成し、正しい issue と関連付けてください。
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報の収集や操作を実行できるようにします。GitHub ツールを初めて使うときに、GitHub への認証を求められる場合があります。画面の指示に従ってください。
5. Copilot が issue を検索して該当するものを見つけ、PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択して PR を確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- モデル コンテキスト プロトコル (MCP) とは何か、MCP サーバーが Copilot に新しいツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- Playwright MCP サーバーを使って、リリース前にブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成しました。

MCP サーバーを使って機能をテストしました。MCP は Copilot を拡張する方法の 1 つです。次は[カスタム エージェントを作成し][next-lesson]、アクセシビリティ レビューのような対象を絞ったタスクを効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP レジストリ][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
