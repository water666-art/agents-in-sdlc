---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前の演習: 前提条件][previous-lesson] · [次の演習: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」作業するかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに適用範囲を限定した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。

> [!NOTE]
> VS Code やコマンドラインインターフェイス (CLI) のハーネスとは異なり、ここではプロンプトの「実行前と実行後」を比較しません。Copilot cloud agent は GitHub Issue に対して非同期に動作するため、リアルタイムで横並びの比較を示しにくいためです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

優れた開発会社である Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データ層には必ずユニットテストを用意します。
- ユーザーインターフェイス (UI) はダークモードにし、モダンな印象にします。
- コードのドキュメントは、TypeScript ドキュメントコメント (TSDoc) として追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot がこれらのプラクティスに沿って作業できるように、必要な情報を提供します。

### カスタム指示

カスタム指示を使うと、Copilot がコーディングスタイルや要件を理解しやすくなるように、コンテキストや好みを伝えられます。希望するコーディング規約、ライブラリ、含めたいコメントの種類などを指定して、より適切な提案やコードスニペットを得られます。リポジトリ全体、または特定の種類のファイルに対するタスク単位のコンテキストとして、指示を作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。Copilot Chat や CLI でのリクエストの多くに関係する、プロジェクトの技術スタック、構築するものの概要、ベストプラクティス、グローバルなガイダンスなどを記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類に合わせて作成できます。TypeScript や Astro などの特定の言語、または UI コンポーネントや新しいユニットテストの作成などのタスクにガイダンスを提供できます。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot のコマンドラインインターフェイス (CLI)、Copilot cloud agent は、リポジトリレベルの指示ファイルと、`applyTo` フロントマターを含む `*.instructions.md` ファイルの両方を使います。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく指示ファイルにも対応しています。[Copilot の指示ファイルのサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することはこの演習の範囲外ですが、サンプルプロジェクトには代表的な方法が示されています。基本的な指針は次のとおりです。

- `copilot-instructions.md` には、構築するものやプロジェクトの構造、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルは、ユニットテスト、Astro コンポーネント、データ層などのファイル形式や、特定のタスクに合わせて使います。
- 自然な言葉で、明確なガイダンスとコードの例を提示します。コードの望ましい例と避けるべき例を示してください。

指示ファイルの作成方法は一つではありません。AI の使い方と同様に、試しながらプロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルのセットを用意することをお勧めします。このプロジェクトの指示ファイルを確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク用のファイルがあります。
>
> Copilot を使って指示ファイルを生成することもできます。操作方法は利用する環境によって異なります（たとえば、VS Code のユーザーインターフェイス (UI) にある **Configure Chat → Generate Agent Instructions** や、Copilot CLI の `/init` など）。関連する演習では、該当する操作を説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースを集めたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれている指示ファイルを読みます。中核となる `copilot-instructions.md` が 1 つあり、さまざまなタスクに対応する `*.instructions.md` ファイルもあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の中にある **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot を使うすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドを確認してください。このフィールドには、リポジトリルートからの相対パスで、指示が適用されるファイルを指定する glob が設定されています。たとえば、`**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などのほかの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。これにより、大きな指示セットを再利用できる小さなファイルに分割し、コード生成時に参照する例を Copilot に示せます。リンク先のパスは、リポジトリルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc のドキュメントコメントとファイルコメントヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は 1 つです。コード内のドキュメントを TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーとして記載するプロジェクト全体のルールを追加します。このハーネスの後半で Issue を割り当てると、cloud agent はこのルールを適用します。

編集する前に、作業用のブランチを作成します。cloud agent は Issue の対象ブランチにある指示を読み取りますが、変更を明確に分けるため、ここではブランチを作成してコミットします。

1. codespace のターミナルで、次のコマンドを実行して新しいブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に提供するファイルです。リポジトリのコーディング標準にルールを追加し、コード提案の品質を高めます。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が説明されていますが、コード内のドキュメントに関するルールはまだありません。そのため、生成されたヘルパー関数にはドキュメントコメントがありませんでした。
3. 既存の標準のすぐ下に、次の Markdown 行を追加します。これにより、Copilot にファイルコメントヘッダーと TSDoc のドキュメントコメントを追加するよう指示できます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前の演習で確認したとおり、指示ファイルはグローバルなガイダンスを記載するリポジトリレベルの `.github/copilot-instructions.md` と、特定の言語、ファイル形式、タスクに対応する `*.instructions.md` ファイルのどちらにも作成できます。今回追加したドキュメントコメントのルールのような、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、Issue の対象ブランチにある指示ファイルを読み取ります。次の演習で Copilot に Issue を割り当てるとき、Copilot は `main` からブランチを作成します。cloud agent が指示を利用できるように、変更を `main` に取り込む必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成し、マージします。次の演習で割り当てる Issue に取り組む際、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチを作成せずに `main` にコミットしてもかまいません。ここでブランチを作成するのは、実際のプロジェクトで行う方法を再現するためです。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、リポジトリ全体に適用する新しい標準を `.github/copilot-instructions.md` に追加しました。このハーネスを進めると、cloud agent が作成するプルリクエストで、その標準が適用されていることを確認できます。

次に、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示をより適切に作成する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
