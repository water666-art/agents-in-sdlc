---
title: "レッスン 2 - 星評価を追加する：小さな成果"
description: "Copilot CLI でゲームカードに小さな変更を加え、転送されたブラウザーで確認して、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次は、プロジェクトに最初の変更を加えます。変更は小規模です。ゲームのデータにはすでに星評価が含まれていますが、ホームページのゲームカードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次の作業を行います。

- 機能ブランチで Copilot との会話を開始します。
- プロジェクトに小さな変更を加えるようエージェントに依頼します。
- `/diff` で変更をレビューします。
- アプリを実行し、転送されたブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価を設定でき、ゲーム詳細ページにはすでに表示されています。一方、ホームページのゲームカードには、タイトル、カテゴリー、パブリッシャー、説明だけが表示されています。最初の作業として、各カードに既存の評価を表示するようエージェントに依頼します。小規模で自己完結した、最初のセッションに最適な変更です。

## 会話の構成

**会話**では、Copilot CLI を使ってタスクに取り組みます。Copilot app とは異なり、通常の CLI 会話では専用の worktree を作成せず、ターミナルで現在チェックアウトされているリポジトリと Git ブランチを使用します。保存した会話を後から再開できます。ファイルとブランチはディスク上の通常の Git 状態として保持されます。

会話では、プロンプトとエージェントの応答、ファイルを調査・編集するエージェントのツールアクティビティ、`/diff` で確認できる変更の3つを扱います。

## 会話を開始して変更を依頼する

新しい会話を開始し、機能の実装に取りかかります。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. まだ Copilot を起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュコマンド `/new` を実行し、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトを使って変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調査し、ゲームの詳細表示に使われるファイルを見つけ、必要なコードを作成します。これで Copilot CLI を使って新機能を追加できました。

## 差分をレビューする

AI が生成した変更は、どれほど小さくてもマージ前にレビューします。Copilot CLI で変更を確認してください。

1. `/diff` を入力して、変更されたすべてのファイルを確認します。
2. 評価があるゲームカードには数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. どちらの状態もテストされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら、<kbd>Esc</kbd> を押して差分画面を終了します。

> [!NOTE]
> Copilot はすべての生成 AI ツールと同様に確率的に動作するため、コードは例と異なる場合があります。特定の実装と一致するかではなく、動作をレビューしてください。

## 変更を確認する

コードを読むだけで動作すると判断せず、Copilot にアプリを起動させ、Codespaces から転送されたブラウザーで更新されたユーザーインターフェイス (UI) を確認します。

1. 次のプロンプトで Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択します。
3. ゲームカードに5点満点の評価が表示されることを確認します。
4. Copilot に戻り、次のプロンプトで起動したサーバーの停止を依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次は、プロジェクトに新しいコードをマージするプルリクエスト (PR) を作成します。

1. 既定のエージェントに、次のプロンプトを使って変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。PR の作成時に Copilot CLI が既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI に表示された URL を選択します。Mac では <kbd>Command</kbd> キー、Windows/Linux では <kbd>Ctrl</kbd> キーを押しながら選択してください。
4. 変更されたファイルとチェックをレビューします。
5. 準備ができたら **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って最初の変更をリリースしました。具体的には、次の作業を行いました。

- 機能ブランチで Copilot との会話を開始しました。
- ゲームカードに小さな変更を加えるようエージェントに依頼しました。
- `/diff` で変更をレビューしました。
- アプリを実行し、転送されたブラウザーで星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は、[フィルタリングの Issue から始めて Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
