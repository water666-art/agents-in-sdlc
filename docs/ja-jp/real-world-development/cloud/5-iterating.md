---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前の演習: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスの改善や機能追加を目的とした複数のタスクについて、GitHub Copilot と作業してきました。コードにドキュメントを追加し、デザインチームが改善を検討できる関連ゲーム機能を構築し、ハイコントラストモードとライトモードの切り替えを含むアクセシビリティ機能を実装するよう Copilot に依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを返して作業を改善しましょう。

### シナリオ

これまで何度も確認したとおり、生成 AI を導入しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードを必ずレビューし、通常の DevOps プロセスに沿って作業します。チーム全体でレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認します。
## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は、監督を受けずに非同期でタスクを実行するため、安全性を確保するためのセキュリティ上の制約があります。たとえば、次のとおりです。

- Copilot はリポジトリに対する読み取りアクセス権を持ち、コードの作業に使うブランチに対してのみ書き込みアクセス権を持ちます。
- cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- [外部リソースへのアクセスはデフォルトで制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。モデルコンテキストプロトコル (MCP) サーバーも対象です。
## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。この作業では、GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールによってブロックされたという警告が表示される場合があります。Copilot は外部 MCP サーバーへの呼び出しなど、外部リソースへのアクセスがデフォルトで制限されているため、これは**想定された動作**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエスト一覧を開きます。
3. **Add missing documentation** などのタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** フラグが付いています。その場合は、Copilot が作業を完了するまで待ってください。数分かかることがあります。休憩したり、ここまでに学んだことを振り返ったりしてもよいでしょう。

4. プルリクエストの準備ができたら、**Files changed** タブを選択して変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 更新されたコードを確認します。新しく作成された TypeScript ドキュメントコメント (TSDoc) や、その他のドキュメントが含まれます。具体的な変更内容は異なります。

    変更を確認するときは、TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーを探します。これらは、演習 2 の最初に確認したカスタム指示ファイルによるものです。

6. 更新内容をレビューし、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. 承認待ちのワークフローがあることを示すインジケーターを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. プルリクエストのチェックセクションで、ワークフローがキューに入ったことを確認します。正常に進めば、単一の Astro アプリのプロジェクトチェックが通ります。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストでの Copilot との作業は一方通行ではありません。チームメンバーと同様に、プルリクエストのコメントやコードへのインラインコメントで Copilot にメンションできます。Copilot はそのコメントを確認し、対応するための新しいセッションを開始します。結果は決定的ではないため、どのような依頼をすべきかを一つに定めることはできません。たとえば、次のような更新を依頼できます。

- 各コードファイルの先頭に、そのファイルの概要を示すコメントヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例、またはコードベースに追加したいドキュメントに関する別の案を使い、**@copilot** にメンションして、生成されたドキュメントの変更をコメントで依頼します。
2. **View Session** を選択して、Copilot が作業する様子を確認します。Copilot が更新のために新しいセッションを開始することに注目してください。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで、Copilot との反復を続けてください。準備ができたら、ドラフト PR をレビュー可能な状態に変更し、`main` ブランチにマージできます。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次に、ゲーム詳細ページに関連ゲームを表示する Issue について、Copilot が作成した PR に戻ります。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容をレビューし、問題がなければ **Conversation** タブに戻って下にスクロールします。
6. 承認待ちのワークフローがあることを示すインジケーターを確認します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. プルリクエストのチェックセクションで、ワークフローがキューに入ったことを確認します。正常に進めば、単一の Astro アプリのプロジェクトチェックが通ります。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストすることもできます。Codespace に移動し、ターミナルを開いて次のコマンドを実行します（`<branch-name>` を Copilot が作成したブランチ名、たとえば **copilot/fix-8** に置き換えてください）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```
   
Copilot は関連ゲーム機能を作成しました。前回と同様に、Copilot cloud agent に更新を依頼して反復できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc のドキュメントコメントが追加されていることを確認したりできます（この Issue はカスタム指示を更新する**前**に割り当てたことを思い出してください）。前回と同様、**Conversation** タブに新しいコメントを追加して依頼できます。Copilot はそのコメントを確認し、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントを使って実装したアクセシビリティ機能をレビューします。この PR には、演習 3 で割り当てたハイコントラストモードと、演習 4 のミッションコントロールから依頼したライトモードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** フラグが付いています。その場合は、Copilot が作業を完了するまで待ってください。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注目してください。
   - モードの切り替えに使うトグルユーザーインターフェイス (UI) コンポーネント
   - ユーザー設定の保存に使うローカルストレージ
   - ハイコントラストモードとライトモードのカスケーディングスタイルシート (CSS) またはスタイルの変更
   - アクセシビリティ属性（Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作など）
   - モードの切り替えを管理する JavaScript または TypeScript コード

6. 更新内容をレビューし、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. 承認待ちのワークフローがあることを示すインジケーターを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. プルリクエストのチェックセクションで、ワークフローがキューに入ったことを確認します。正常に進めば、単一の Astro アプリのプロジェクトチェックが通ります。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて次のコマンドを実行します（`<branch-name>` を Copilot が作成したブランチ名に置き換えてください）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次に、アプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ用のカスタムエージェントが、アクセシビリティのベストプラクティスに沿って機能を実装するよう Copilot を導いたことを確認してください。アクセシビリティに関する懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** にメンションして、更新を依頼できます。

## 省略可能な演習 — 委任を続ける

実際のバックログ項目を任せて別の作業に移れるとき、cloud agent は特に役立ちます。この方法を習慣にするため、リポジトリにさらにいくつかの Issue を登録し、Copilot に割り当ててください。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向け関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス用ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各演習を通じて、次のことを行いました。

- このリポジトリに含まれるカスタム指示ファイルを確認し、後で cloud agent の出力に反映される様子を確かめました。
- Issue を Copilot cloud agent に割り当て、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタムエージェントを作成して使い、ハイコントラストモードとライトモードの切り替えを追加しました。
- Copilot エージェントページをミッションコントロールとして使い、アクセシビリティのセッションを監視して方向付けました。
- cloud agent のプルリクエストをレビューして反復し、**@copilot** に変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに学習を続ける場合は、ほかのハーネスで今回の内容を発展させることができます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — 統合開発環境 (IDE) で Copilot Chat のエージェントモードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — コマンドラインインターフェイス (CLI) から Plan モードと Autopilot モード、エージェントスキル、カスタム品質保証 (QA) エージェント、Playwright MCP、Agent Merge、cloud agent に戻るオプションの `/delegate` ブリッジを使って、レビュー済みの変更を 2 つ作成します。

自分のリポジトリでは、次のアイデアも試してみてください。

- リファクタリングやテストカバレッジに関する Issue を cloud agent に割り当てます。
- 別の分野向けのカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前の演習: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
