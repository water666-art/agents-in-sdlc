---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成人工知能 (AI) を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。その中でも重要なのが[指示ファイル][instruction-files]です。指示ファイルを使って、コード生成に関するガイダンスを提供します。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス指定の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 現在の指示を使って、フィルタリング機能の最初のデータ部分（出版社用ヘルパー）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。
- 同じプロンプトをもう一度実行し、生成コードに新しい標準が反映される様子を確認します。
- 指示の更新とフィルタリング機能の一部を `main` にコミットし、次の演習で Copilot が更新済みのガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準と異なる場合があります。Copilot の動作は確率的です。この演習では、出力を一字一句一致させるのではなく、指示の更新後に動作がどのように変化するかを確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発作業に関するガイドラインと要件があります。たとえば、次のとおりです。

- データ層には必ず単体テストを用意します。
- ユーザー インターフェイス (UI) はダークモードにし、現代的な雰囲気にします。
- TypeScript のドキュメント コメント (TSDoc) を使ってコードに説明を追加します。
- 各ファイルの先頭に、そのファイルの機能を説明するコメント ブロックを追加します。

指示ファイルを使うと、Copilot がこれらの開発プラクティスに沿ってタスクを実行できるよう、必要な情報を提供できます。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストと希望を Copilot に伝えられます。これにより、より適切な提案やコード スニペットを得られます。希望するコーディング規約、ライブラリ、コメントの形式などを指定できます。リポジトリ全体、または特定のファイルの種類に対して指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、このリポジトリに対する**すべての**リクエストで Copilot に送られる単一の指示ファイルです。Copilot Chat やコマンドライン インターフェイス (CLI) である Copilot CLI でのほとんどのリクエストに関係する、プロジェクトの技術スタック、開発内容の概要、ベスト プラクティスなど、プロジェクト全体に関する情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類に関するガイドラインを提供します。TypeScript や Astro などの言語や、UI コンポーネントや単体テストの作成などのタスクを対象にできます。

> [!NOTE]
> 統合開発環境 (IDE) で指示ファイルがコード生成に使われるのは Copilot Chat のみです。コード補完や次の編集の提案には使われません。
>
> コード生成時には、Copilot Chat、Copilot CLI、Copilot cloud agent が、リポジトリ レベルの指示ファイルと `applyTo` front matter を含む `*.instructions.md` ファイルの両方を使います。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく指示ファイルにも対応しています。[指示ファイルの対応状況][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成に関する詳しい説明はこのワークショップの範囲外ですが、サンプル プロジェクトには一般的な方法を示す例があります。基本的には、次の点に留意してください。

- `copilot-instructions.md` には、作成中のものやプロジェクト構成、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ファイルの種類（単体テスト、Astro コンポーネント、データ層など）や特定のタスクに関する指示を記載します。
- 自然な言葉を使い、分かりやすいガイダンスと、適切なコードおよび不適切なコードの例を示します。

指示ファイルの書き方に唯一の正解はありません。AI の使い方と同じく、プロジェクトで試しながら最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実した指示ファイルを用意することをお勧めします。このプロジェクトの指示ファイルを見ると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用のファイルがあることに気付くでしょう。
>
> Copilot に指示ファイルを生成してもらうこともできます。各環境での方法は異なります。たとえば、VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。該当する場合は、使用中の環境に関するレッスンで方法を説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタム エージェント、その他のリソースが集められたリポジトリ [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。中核となる `copilot-instructions.md` が 1 つあり、さまざまなタスク用の `*.instructions.md` ファイルもあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明に加え、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** にある **GitHub Actions Workflows** のガイダンスにも注目してください。これらは Copilot とやり取りする際に適用されます。
3. `.github/instructions` フォルダーを開き、内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭にある `applyTo` フィールドを確認してください。このフィールドは、指示が適用されるファイルをリポジトリのルートからの glob で指定します。この例では、`**/*.test.ts` に一致するものなど、すべての TypeScript テスト ファイルが対象です。
5. このプロジェクトで単体テストを作成するための指示を確認します。
6. 最後に、`.github/instructions/drizzle.instructions.md` を開いて一番下までスクロールします。`unit-tests.instructions.md` などのほかの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。これらのパスはリポジトリのルートではなく、指示ファイルの場所を基準にしています。大きな指示セットを再利用可能な小さなファイルに分け、コード生成時に従う例を Copilot に示せます。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc ドキュメント コメントとファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ここでは、変更を別のブランチで行う方法を確認します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。使用するモードとモデルは演習ごとに異なるため、必要な場合は各手順で指定します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送ります。後で指示ファイルを更新し、同じプロンプトをもう一度送って違いを確認します。

1. 前の演習で開いたエディター タブをすべて閉じ、Copilot が必要なコンテキストだけを参照できるようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   src/lib/publishers.ts を作成または更新し、各出版社の名前と識別子 (ID) を含む一覧を返すデータアクセス ヘルパーを実装してください。ファイルに変更を適用してください。
   ```

5. Copilot はプロジェクトを調べ、ヘルパー ファイルとテストなどにコードの更新を適用します。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取り、型付きの出版社配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層に関する規約に従っています。
7. 提案されたコードに TSDoc ドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> 確率的に動作するため、指示がなくても Copilot がドキュメント コメントを追加する場合があります。その場合も問題ありません。指示を更新した後に一貫性が向上することが、この演習の目的です。

## 新しいリポジトリ標準を追加する

前述のとおり、`.github/copilot-instructions.md` にはプロジェクト レベルの情報を記載します。リポジトリのコーディング標準を記載して、コード提案の質を高めます。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。プロジェクトのコーディング標準は記載されていますが、コード内のドキュメントに関するルールがないため、生成されたヘルパーにはドキュメント コメントがありませんでした。
3. 既存の標準のすぐ下に、次の Markdown 行を追加します。これにより、Copilot にファイル コメント ヘッダーと TSDoc ドキュメント コメントの追加を指示します。

   ```markdown
   - エクスポートするすべての関数に、目的、パラメーター、戻り値を説明する TSDoc コメントを付ける。
   - import やコードより前に、そのファイルの目的を説明するコメント ブロックを追加する。
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ レベル (`.github/copilot-instructions.md`) に作成して全体的なガイダンスを示すことも、特定の言語、ファイルの種類、タスク用に `*.instructions.md` ファイルとして作成することもできます。今回追加したドキュメント コメントのルールのようなプロジェクト全体の標準は、リポジトリ レベルのファイルに記載します。

## プロンプトをもう一度実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が対象ファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   src/lib/publishers.ts を作成または更新し、各出版社の名前と識別子 (ID) を含む一覧を返すデータアクセス ヘルパーを実装してください。ファイルに変更を適用してください。
   ```

4. 今度は、次の例のようなコメント ブロックで提案されたファイルが始まることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディング プラットフォームの出版社データアクセス用ヘルパーです。
    * データベースから出版社の情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次の例のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * ID と名前を含むすべての出版社の一覧を返します。
    *
    * @param db - Drizzle データベース クライアントです。
    * @returns 出版社オブジェクトの配列に解決される Promise です。
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で使う実際のコードに適用しました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもリポジトリ内のほかのファイルと同様に、ソース管理を使って管理します。指示の更新とブランチをリポジトリにコミットしてプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナル ウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで、指示の更新とヘルパーの基盤に関して Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加の関連変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルで、ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたので、プルリクエスト (PR) を作成し、バックログにあるドキュメント標準の issue と関連付けます。手作業でもできますが、GitHub のツールがプロジェクトに接続されているため、Copilot に依頼して issue を見つけ、issue をクローズする PR を作成し、マージできます。

> [!NOTE]
> この演習では、初めて Copilot に GitHub 上の操作を任せます。プロジェクト テンプレートでは GitHub のツールがすでにワークスペースに接続されているため、セットアップは不要です。Copilot が初めてツールを使うときに、VS Code から GitHub へのサインインを求められる場合があります。指示に従って許可してください。この接続の仕組みである Model Context Protocol (MCP) については、後の演習で説明します。

1. codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関連する issue を見つけ、現在のブランチから PR を作成するよう Copilot に依頼します。PR には、指示の更新と、今後のフィルタリング作業用に追加した出版社ヘルパーの基盤の両方を記載します。

   ```
   指示ファイルの更新に関連する issue を見つけてください。現在のブランチから新しい PR を作成し、その issue をクローズすることを明記してください。また、今後のフィルタリング作業用に出版社ヘルパーの基盤も追加したことを記載してください。
   ```

4. Copilot が issue の検索と PR の作成を開始します。
5. Copilot に代わって GitHub の操作を許可するよう求められたら、コマンドを確認し、適切な場合は **Allow** を選択します。
6. PR が作成されたら、次のプロンプトで PR をマージし、ブランチをローカルの `main` に戻すよう Copilot に依頼します。

   ```
   PR を main にマージしてください。その後、ローカルで main に戻り、最新のコードを取得して最新状態にしてください。
   ```

7. Copilot に代わって GitHub の操作とシェル コマンドの実行を許可するよう求められたら、コマンドを確認し、適切な場合は **Allow** を選択します。

GitHub Copilot を使って PR を作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat を使って次のことを行いました。

- コード生成用のプロンプトを送り、*既存の*指示に基づいて Copilot が生成する内容を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再度送り、提案されたコードに新しい標準が反映されることを確認しました。
- 指示の更新とヘルパーの基盤を `main` にコミットし、次の演習の土台を作りました。

次は[エージェントモード][next-lesson]で指示を活用し、Copilot にコードベース全体にまたがる新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 用のカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
