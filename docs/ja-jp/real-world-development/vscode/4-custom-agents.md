---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや領域に合わせた専門的な人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、対象の作業をより効果的に進めるための具体的な指示、ベスト プラクティス、コーディング パターン、ドメイン固有の知識を Copilot に提供します。チームは専門知識を再利用可能なエージェントとして形式化できます。たとえば、[Web コンテンツ アクセシビリティ ガイドライン (WCAG)][wcag] への準拠を確認するアクセシビリティ エージェント、セキュア コーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントなどがあります。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタム エージェントとエージェント スキルの比較

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、概念的に重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。最も分かりやすく区別するなら、**カスタム エージェント**は作業者で、**スキル**はツールです。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業を進めるためにスキル（ほかのエージェントも含む）をオーケストレーションするよう設計されています。このラボでは、アクセシビリティ用カスタム エージェントがガイドラインに照らしてサイトをレビューし、更新します。その作業の一環として、プルリクエスト ワークフロー用のスキルや、テストを実行、管理するスキルなどを呼び出すことがあります。

> [!NOTE]
> カスタム エージェントの作成に唯一の「正しい」方法はありません。AI のほかの機能と同じように、環境やシナリオに合う方法を見つけるため、テストと反復を行ってください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるように取り組んでいます。最近のユーザー フィードバックでは、テキストと背景色のコントラストが不十分なため、現在のダークテーマが読みにくいという指摘がありました。このアクセシビリティ上の懸念に対応するため、デザイン チームはユーザーが切り替えられる高コントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。カスタム エージェントを使って機能を生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ用カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、高コントラストモードを実装します。

## アクセシビリティ用カスタム エージェントを確認する

アクセシビリティ用カスタム エージェントはすでに用意されています。Copilot の作業をどのように導くか理解するため、内容を確認します。

codespace に戻り、ターミナルを開いて、アクセシビリティ作業用に `main` から新しいブランチを作成して切り替えます（演習 3 のフィルタリング PR とは分けます）。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` を含む frontmatter は、カスタム エージェントに必須です。

3. 続いて、次の内容を説明する各セクションを読みます。
   - アクセシブルな Web サイトのコードを生成する際の中核的な責務。
   - アクセシビリティのベスト プラクティス。
   - ハイパーテキスト マークアップ言語 (HTML)、カスケーディング スタイル シート (CSS)、JavaScript のコード例。
   - よくある落とし穴や間違いの一覧。

## Copilot Chat でカスタム エージェントを使う

VS Code では、`.github/agents` に定義されているすべてのカスタム エージェントが、Copilot Chat ビュー下部のエージェント ドロップダウンに表示されます。カスタム エージェントを選択すると、そのエージェントの指示とツールに合わせてチャット セッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。使用するモードとモデルは演習ごとに異なるため、必要な場合は各手順で指定します。

1. Chat ビューのエージェント ドロップダウンで、まだ選択されていない場合は **Agent** を選択します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント ドロップダウンを選択します。選択中のエージェントが表示されています。既定では **default** です。
3. 利用できるエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

    ```plaintext
    サイトに高コントラストモードを追加してください。ユーザーが高コントラストを切り替えられるようにし、設定をブラウザーの local storage に保存して、ページを再読み込みしても維持してください。
    ```

5. Copilot Chat がコードベースを調べ、変更を提案し、プロジェクト ファイルに適用します。各編集はファイル パスと差分とともにチャット内に表示されます。
6. 編集が反映されると **Files changed** の表示が更新され、エージェントが変更したファイルを確認できます。

> [!NOTE]
> この処理には数分かかることがあります。Copilot はリポジトリに実際の変更を加えています。作業中に Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

進行中の作業のレビューと方向付けは、次の演習で行います。

## まとめと次のステップ

このレッスンでは、特定のタスクや領域に合わせた専門的な AI アシスタントである GitHub Copilot の[カスタム エージェント][custom-agents]を確認しました。カスタム エージェントを使うと、チームの専門知識や標準を再利用可能なエージェントとして形式化し、特定の種類の作業をより効果的に進めるよう Copilot を導けます。

このレッスンでは、次の内容を確認しました。

- カスタム エージェントの定義方法。
- Copilot Chat のエージェントモードでカスタム エージェントを使う方法。

次は[エージェントの作業を監視して方向付け][next-lesson]、変更を確認しながら、同じセッションにライトモード切り替えを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [統合開発環境 (IDE) でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
