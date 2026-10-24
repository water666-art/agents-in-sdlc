---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前の演習: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織はほとんどありません。解決されていないセキュリティ上の問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行します。エージェントが作業を終えると、人間の開発者がレビューできるよう、ドラフトのプルリクエスト (PR) を作成します。面倒な作業を任せて開発を加速し、開発者がより大きな課題に集中できるようになります。

GitHub Copilot cloud agent を使って、次のことを確認します。

- コード生成のための環境をカスタマイズする方法
- 安全に操作を実行する方法
- 明確に範囲を定めた Issue の重要性
- Issue を Copilot に割り当てる方法

## シナリオ

Tailspin Toys は、技術的負債の解消を検討しています。Web サイトの最初のバージョンを作成した請負業者は、ドキュメントを十分に整備していませんでした。実際、ドキュメントはほとんどありません。まず、アプリケーション内のエクスポートされた関数すべてに TypeScript ドキュメントコメント (TSDoc) を追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのゲーム、つまり関連ゲームを表示し、閲覧を続けられるようにしたいと考えています。まずはユーザーエクスペリエンス (UX) の受け入れテストに使える実装があれば十分です。現在、この作業はブロッカーになっていますが、優先度がより高い Issue もあります。

どちらも優先順位が下がりやすいタスクの例であり、Copilot cloud agent に割り当てるのに適しています。Copilot cloud agent に非同期で作業させている間、開発者はほかのタスクに取り組み、完了後に戻って作業内容をレビューし、期待どおりであることを確認できます。
## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。人間の開発者と作業する場合と同様に、[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、複数の方法で作業を依頼できます。割り当てると、Copilot は進捗を追跡するドラフトのプルリクエストを作成し、環境をセットアップして、タスクに取り組み始めます。作業中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決策をレビューできる状態になると、プルリクエストで通知されます。
## 適切な範囲を定めた指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見単純な操作でも、詳しく見ると複雑な場合があります。そのため、[Copilot cloud agent に依頼するタスクの範囲には注意してください][cloud-agent-best-practices]。AI ペアプログラマーとして段階的に作業し、学び、試し、必要に応じて調整してください。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と、それが Copilot を導く仕組みを確認しました。カスタム指示の演習を終えている場合は、`.github/copilot-instructions.md` が機能する様子を見たことがあるでしょう。まだの場合は、ここで簡単に確認してください。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれている指示ファイルを読み取り専用で確認し、後でその影響を確かめられるようにします。

リポジトリの GitHub Web UI、または起動済みの codespace で、次のファイルを開いてください。

- `.github/copilot-instructions.md` — **Code standards** セクションを確認します。特に TypeScript の規約と TypeScript ドキュメントコメント (TSDoc) に関する要件に注目してください。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成されたプルリクエストで TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーを確認してください。これらは、指示ファイルによって適用されます。後の演習で PR をレビューするときに、もう一度確認します。

## Copilot cloud agent の開発環境をセットアップする

誰がコードを書く場合でも、通常は適切な状態にするための環境やセットアップスクリプトが必要です。Copilot にタスクを割り当てる場合も同様です。Copilot はソフトウェアエンジニアと同じような方法で作業します。

cloud agent は作業環境として [GitHub Actions][github-actions] を使用します。`.github/workflows/copilot-setup-steps.yml` ファイルで設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に実行する処理をカスタマイズできます。これにより、必要な開発ツールや依存関係を利用できるようになります。このラボでは、手順を簡潔にし、学習に集中できるよう、事前に設定されています。Cloud agent が Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一の Astro アプリ用に移行とシードが済んだ SQLite データベースを利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできるようにします。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # セットアップ手順に必要な最小限のアクセス許可を設定します。Copilot の操作には独自のトークンが使われます。
    permissions:
      # たとえば依存関係をインストールするためセットアップ手順でリポジトリをクローンする場合は、`contents: read` のアクセス許可が必要です。セットアップ手順でクローンしない場合、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカルの SQLite データベースを移行してシードを投入し、ビルドとテスト用のデータを準備します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローファイルと似ていますが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot がプルリクエストに取り組む前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot の実行を待たずに、ワークフローが正常に動作するかテストする場合に便利です。

## ドキュメントを追加する

誰もがドキュメントの重要性を理解していますが、多くのプロジェクトでは情報が古かったり、ドキュメント自体がなかったりします。このような技術的負債は解消されないままになりやすく、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成に役立つため、Copilot cloud agent に割り当てるのに適した Issue です。バックグラウンドで必要なドキュメントを作成します。後の演習で、その作業をレビューします。

1. 新しいブラウザータブで、github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue ダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    組織では、役立つ場合に関数やメソッドへ TSDoc のドキュメントコメントを付けることが求められています。しかし、最近の更新ではこの標準が守られていません。動作を明確にする既存のコードにドキュメントコメントを追加してください。
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

  ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエストを開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。一覧に新しい PR が表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後に、Copilot が todo リストを作成したことを確認できます。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかる場合があります。Copilot は環境を作成し（前述のワークフローを実行します）、プロジェクトを分析して、問題に取り組む最善の方法を決めています。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された状態を確認するために、ウィンドウを更新する必要がある場合があります。

16. 実行中のセッションをスクロールして、Copilot が問題に取り組む様子を確認します。コードを調査して状態を理解し、計画を検討して決定し、コードを作成する過程を確認できます。

この作業には数分かかる場合があります。Copilot cloud agent の主な目的の 1 つは、タスクを非同期で実行し、開発者がほかの作業に集中できるようにすることです。この機能を活用し、Copilot cloud agent にもう 1 つのタスクを割り当てた後、アプリケーションに機能を追加するコード作成に取り組みます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の大きな利点の 1 つは作業を分担できることです。デザインチーム向けに関連ゲームのセクションを追加する作業は時間がかからないとしても、その時間を別のタスクに使えます。Copilot cloud agent に割り当ててみましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue ダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    ゲームの詳細ページに関連ゲームを表示して、ほかのゲームも見つけやすくしたいと考えています。デザインチームは UX を検討し、受け入れテストを行う予定です。要件は次のとおりです。

   - `src/lib/` にデータアクセス用ヘルパーを追加します。このヘルパーは、指定されたゲームを除き、同じカテゴリに属するほかのゲームを返します。
   - ゲーム詳細ページにヘルパーを使った「関連ゲーム」セクションを表示します。
   - 関連ゲームがない場合にも対応します。
   - 新しいヘルパーのユニットテストを作成します。
   - PR を作成する前に、すべてのテストが通ることを確認します。
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![Issue に表示された目の絵文字は、Copilot が作業中であることを示します](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じような方法で作業するため、常に監視する必要はありません。完了後にレビューできます。次に、カスタムエージェントの作成と使用について確認します。

## まとめと次のステップ

この演習では、[GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に Issue を割り当てると、非同期に処理できます。技術的負債の解消、新機能の作成、あるフレームワークから別のフレームワークへのコード移行などに Copilot を活用できます。

次の概念を確認しました。

- コード生成のための環境をカスタマイズする方法
- 安全に操作を実行する方法
- 明確に範囲を定めた Issue の重要性
- Issue を Copilot に割り当てる方法

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使います。[Copilot cloud agent ではモデルコンテキストプロトコル (MCP) サーバーも利用できます][cloud-agent-mcp]。また、前の演習で確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次の演習: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
