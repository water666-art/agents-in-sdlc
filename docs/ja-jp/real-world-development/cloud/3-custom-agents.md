---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせて、専門的な人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成してエージェントを定義し、重点を絞った指示、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に提供すると、特定の作業をより効果的に実行できるようになります。チームは専門知識を再利用可能なエージェントにまとめられます。たとえば、Web Content Accessibility Guidelines (WCAG) 準拠を徹底するアクセシビリティエージェント、安全なコーディングプラクティスに従うセキュリティエージェント、一貫したテストパターンを維持するテストエージェントなどです。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー内の Markdown ファイル、またはグローバルな `~/.copilot/agents` フォルダー内のファイルとして定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定める Markdown プロンプトが含まれます。

### カスタムエージェントとエージェントスキルの比較

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、ある程度の共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。最も簡単な区別の仕方は、**カスタムエージェントは作業者、スキルはツール**というものです。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一部としてスキルやほかのエージェントを調整して使うように設計されています。このラボでは、アクセシビリティ用のカスタムエージェントがアクセシビリティガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエストのワークフロー用スキルや、テストを実行して管理するスキルなどを呼び出すことができます。

> [!NOTE]
> カスタムエージェントを作成する方法に唯一の正解はありません。AI のほかの活用方法と同じように、利用環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタムエージェントを使って、次のことを確認します。

- カスタムエージェントの定義方法
- カスタムエージェントへのタスクの割り当て方法

## シナリオ

Tailspin Toys は、視覚能力や好みに関係なく、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにすることを重視しています。最近のユーザーフィードバックによると、テキストと背景色のコントラストが不十分なため、現在のダークテーマが読みにくいと感じるユーザーがいます。このアクセシビリティの課題を解決するため、デザインチームはユーザーが切り替えられるハイコントラストモードを求めています。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。カスタムエージェントを使って機能を生成します。
## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは、すでに作成されています。Copilot をどのように導くか理解するため、内容を確認します。

codespace に戻り、アクセシビリティ用カスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な責任
   - アクセシビリティのベストプラクティス
   - ハイパーテキストマークアップ言語 (HTML)、カスケーディングスタイルシート (CSS)、JavaScript のコード例
   - よくある問題や誤りの一覧

## Issue を作成して割り当てる

ミッションコントロールは、環境内のすべてのエージェントを操作するための中心的な場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、追加の指示を与えることができます。まず、ハイコントラストモードを作成するタスクを Copilot に割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue ダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    サイトにハイコントラストモードが必要です。ユーザーが切り替えられるスイッチを用意してください。設定はブラウザーのローカルストレージに保存してください。
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択します。
9. カスタムエージェントの一覧から **Accessibility agent** を選択します。

    ![カスタムエージェントと Accessibility が選択された cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクに取り組み始めます。

## まとめと次のステップ

この演習では、特定のタスクや分野に合わせた専門的な AI アシスタントである GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot が特定の作業をより効果的に実行できるように導けます。

次の概念を確認しました。

- カスタムエージェントの定義方法
- カスタムエージェントへのタスクの割り当て方法

Copilot がハイコントラストモードの実装に取り組んでいる間に、次は[ミッションコントロールからエージェントのセッションを監視し、方向付けます][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使う準備をする][org-custom-agents]
- [エンタープライズでカスタムエージェントを使う準備をする][enterprise-custom-agents]

---

| [← 前の演習: GitHub Copilot cloud agent][previous-lesson] | [次の演習: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
