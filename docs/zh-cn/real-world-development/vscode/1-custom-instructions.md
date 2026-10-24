---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能够获取相关上下文。你可以通过多种方式向 Copilot 提供特定上下文，其中重要的一种方式是使用[说明文件][instruction-files]，为代码生成提供指导。

在本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot。
- 在当前说明生效时，生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和筛选数据切片提交到 `main`，供 Copilot 在下一练习中使用更新后的指南。

> [!CAUTION]
> 生成的代码可能不完全符合你设定的某些标准。Copilot 的结果具有非确定性。本练习的重点是观察更新说明后行为的整体变化，而不是要求输出逐字一致。

## 说明文件

### 场景

Tailspin Toys 与其他优秀开发团队一样，有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面（UI）应使用深色模式，并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应有一段注释，说明文件的用途。

通过说明文件，可以确保 Copilot 获取正确的信息，并按照这些实践完成任务。

### 自定义说明

自定义说明可向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。这项功能可以帮助你引导 Copilot 提供更符合需求的建议和代码片段。你可以指定偏好的编码约定、库，甚至代码注释的风格。你可以为整个存储库创建说明，也可以为特定文件类型提供任务级上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：每次向 Copilot 发出存储库请求时都会发送的单个说明文件。文件中应包含项目级信息，即大多数 Copilot 聊天或 CLI 请求都适用的上下文，例如所用技术栈、项目概览、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建的说明文件。可针对特定语言（如 TypeScript 或 Astro）或任务（如创建 UI 组件或一组单元测试）提供指南。

> [!NOTE]
> 在集成开发环境（IDE）中，说明文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云代理在生成代码时会使用存储库级说明以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [还支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本工作坊的范围。不过，示例项目展示了一种有代表性的方法。概括来说：

- 将 `copilot-instructions.md` 中的内容限定为项目级指南，例如项目介绍、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件针对特定文件类型（如单元测试、Astro 组件和数据层）或任务提供说明。
- 使用自然语言，让指南清晰易懂，并提供代码应该和不应该如何编写的示例。

说明文件没有唯一的编写方式，正如 AI 的使用方式也不止一种。通过实践，你会找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的说明文件。查看此项目中的说明文件时，你可能会发现它们涵盖多种任务，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成说明文件。不同界面提供的方式各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前环境对应的课程会在适用时介绍具体方式。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中包含说明文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义说明文件

花些时间阅读此存储库自带的说明文件：一个核心 `copilot-instructions.md` 文件，以及多个用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并查看其中的文件。你会看到 Astro 文件、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob 模式，用于确定说明适用的文件。此处，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会符合条件。
5. 留意此项目中有关创建单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意其中指向其他说明文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明拆分成更小且可复用的文件，并提供示例供 Copilot 参考。（其中的路径相对于说明文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前还没有要求添加代码内文档。接下来的步骤会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为更改创建一个分支。

1. 返回上一练习使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），打开新的终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。不同练习会使用不同模式和模型，相关步骤会在需要时说明。

要观察自定义说明的影响，先在现有说明文件生效的情况下发送提示。稍后更新说明文件后，再次发送相同提示，以便比较差异。

1. 关闭前面练习中打开的编辑器标签页，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应该放在哪里。
3. 在 Chat 视图的代理下拉列表中选择 **Agent**，以便 Copilot 应用文件更改。

   ![显示 Chat 视图中代理选择器的屏幕截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和对应测试。
6. 留意建议的辅助函数是带类型的函数，它将 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这些约定来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层指南。
7. 注意，建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的结果具有概率性，因此它可能会在没有明确要求的情况下添加文档注释。如果发生这种情况也没关系，本练习重点仍然是观察更新说明后的行为一致性。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，以便改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到大约位于第 27 行的 **Code formatting requirements** 部分。留意此处记录了项目的编码标准，但尚未要求在代码中添加文档，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，可以在存储库级别的 `.github/copilot-instructions.md` 中创建全局说明，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供说明。存储库级文件适合记录你刚添加的文档注释规则等项目级标准。

## 重新运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清空缓冲区。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 聚焦于正确文件。
3. 发送与之前**完全相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意建议文件现在以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意建议函数现在包含类似以下的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚才引导 Copilot 遵循新的项目标准，并将其应用于实际代码。下一练习将在此基础上继续。

## 提交说明并推送分支

说明文件与存储库中的其他资产一样，使用相同的源代码管理流程进行管理。现在提交说明更新并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），打开新的终端窗口。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新和 Copilot 为辅助函数基础实现所修改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他辅助更改（例如测试），也要暂存这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在需要创建拉取请求（PR），并将其关联到待办列表中关于文档标准的问题。也可以手动操作，但 Copilot 可以使用已连接到项目的 GitHub 工具代为处理。让 Copilot 查找问题、创建用于关闭该问题的 PR，然后合并。

> [!NOTE]
> 这是首次让 Copilot 代表你在 GitHub 上操作。项目模板已将 GitHub 工具连接到工作区，无需额外设置。Copilot 第一次使用工具时，VS Code 可能会提示你登录 GitHub。请按照提示授权。后续练习会介绍此连接的工作原理（模型上下文协议（MCP））。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认代理选择器中已选中 **Agent**。
3. 让 Copilot 查找与更新说明文件有关的问题，并从当前分支创建 PR，同时说明说明更新和新的发行商辅助函数基础实现：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找问题并创建 PR。
5. 如果 Copilot 提示要代表你执行 GitHub 操作，请先检查命令，再按需选择 **Allow**。
6. PR 创建后，让 Copilot 合并 PR 并将分支切回 main，使用以下提示：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 提示要代表你执行 GitHub 操作和 shell 命令，请先检查命令，再按需选择 **Allow**。

你现在已借助 GitHub Copilot 创建并合并了拉取请求（PR）！

## 总结和后续步骤

你已了解 Copilot 如何从此项目的说明文件获取上下文，然后在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，并观察 Copilot 根据**现有**说明生成的内容。
- 在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。
- 再次运行相同提示，观察建议代码如何采用新标准。
- 将说明更新和辅助函数基础实现提交到 `main`，供下一练习继续使用。

接下来，你将在[代理模式][next-lesson]中应用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [编写有效自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
