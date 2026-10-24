---
title: "レッスン 3 - エージェントモード：Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で Issue に基づくフィルタリング機能を構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に、小さな機能をプロジェクトに追加しました。より大きな変更には、さらに確かなプロセスが必要です。GitHub Copilot CLI は、組織の既存の開発フローに沿って作業できるため、適切な方法で必要な機能を構築できます。このレッスンから、一般的なエージェント主導の開発プロセスを使います。Issue を基に機能を実装し、コードが正しいこと、機能が期待どおりに動くことを確認して、プロジェクトにマージします。

> [!NOTE]
> 機能開発のワークフローでは、同じ会話とブランチを引き続き使います。通常はファイルの種類ごとに別のブランチや PR を使いますが、ここでは中心的な概念に集中するため手順を簡略化します。

このレッスンでは、次の作業を行います。

- GitHub Issue から新しい Copilot の会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この機能開発を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して、QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、利用者はカテゴリーやパブリッシャーでゲームを絞り込める必要があります。バックログの Issue には機能が記載されていますが、複数カテゴリーの組み合わせなど、コーディング前に合意が必要な点もあります。Plan モードで要件を決めてから、範囲を明確にした実装を Autopilot に任せます。

## 背景

AI コーディングエージェントを開発フローに取り入れても、基本的な手順は変わりません。むしろ、これまで以上に重要になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を説明する Issue から始めます。
2. 実装内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションの処理が成功したら、コードをマージします。

> [!NOTE]
> 具体的な手順はチームや組織によって異なりますが、多くの場合は上記の流れを調整したものです。

この標準的な手順に従うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じプロセスで確認されるようにします。

## 会話モード

**会話モード**は、エージェントがどこまで自律的に作業するかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: 開発者とエージェントが一緒に作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントはまず計画を作成します。プロジェクトファイルの編集は行いません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

最初に Plan モードで計画を作成してレビューし、その後 Autopilot で実装します。

## Issue から始める

フィルタリング機能の作業を始める前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新して、フィルタリング作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーを使って **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が追加されます。`#` を使うと、GitHub の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てることで、機能実装やタスク実行の方法を決めてから Copilot に任せられます。複雑な作業では、少し時間をかけて計画することをお勧めします。Plan モードに切り替え、Copilot に計画の作成を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、最適な方法を検討して計画を作成します。

3. フィルタリングの動作について Copilot から質問されたら、希望に基づいて回答します。回答に唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画をスクロールして確認します。
6. 判断と一致しない箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画の作成とレビューが終わったら、Copilot に実装を任せます。Autopilot は、タスクが完了したと判断するまで Copilot が反復して作業できるモードです。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みのバージョンで同様のラベルのオプションを選択します。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って作業を反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画をレビューしてください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードをマージする前にレビューします。他のコードと同様に、変更を確認してサイトを実行し、期待どおりに動くことを確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を Issue および計画時に決めた内容と照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分ビューを終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードがよさそうでも、実際に動くことを確認します。前と同じようにアプリを起動し、Codespaces の転送ポートを使ってサイトを開きます。

1. 次のプロンプトで Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリー、パブリッシャー、それらの組み合わせで絞り込みます。
4. リセット時と結果がない場合の動作が、Issue と合意内容に一致することを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を構築し、レビューしました。このレッスンでは次の作業を行いました。

- GitHub Issue から Copilot の新しい会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、[カスタム指示を使って][next-lesson]コードが文書化された方法に従うようにし、コード生成の仕組みをさらに確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
