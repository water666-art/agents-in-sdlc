---
slug: ja-jp/real-world-development
title: "実践的な開発"
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更を計画し、コードを作成して動作をテストし、プルリクエストをレビューし、エージェントを使って作業を管理する反復的な開発を体験します。

## シナリオ

架空の企業 Tailspin Toys に新しく加わった開発者として作業します。同社は、開発者をテーマにしたボードゲームのクラウドファンディングを提供しています。チームのバックログは GitHub の Issue として登録済みなので、すぐに取りかかれます。バックログには、フィルタリングやページネーションなどの機能追加に加え、アクセシビリティやコーディング規約などの品質改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、普段の作業環境で利用できます。開発方法に合った環境を選んでください。各ワークショップでは個別に環境を設定し、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェントアシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを扱います。

### [GitHub Copilot app][app]

デスクトップアプリでエージェントセッションを並行して実行し、セッションモードを切り替え、キャンバスで共同作業を行い、GitHub の Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub の Issue を非同期エージェントに割り当て、作業の指示や進捗の確認を行い、エージェントが作成したプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを利用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
