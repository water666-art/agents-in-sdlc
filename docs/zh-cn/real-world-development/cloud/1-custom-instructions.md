---
title: "练习 1 - 自定义说明（云代理）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：Copilot 云代理 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能够获取相关上下文。[说明文件][instruction-files]可以提供这些指导，让 Copilot 不仅了解你希望它做什么，也了解你希望它如何完成任务。

在本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，这里不会运行更改前后的提示进行对比。Copilot 云代理会异步处理 GitHub 问题，因此难以实时并排展示其影响。后续你检查云代理生成的拉取请求时，可以看到说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 与其他优秀开发团队一样，有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式，并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应有一段注释，说明文件的用途。

通过说明文件，可以确保 Copilot 获取正确的信息，并按照这些实践完成任务。

### 自定义说明

自定义说明可向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。这项功能可以帮助你引导 Copilot 提供更符合需求的建议和代码片段。你可以指定偏好的编码约定、库，甚至代码注释的风格。你可以为整个存储库创建说明，也可以为特定文件类型提供任务级上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：每次向 Copilot 发出存储库请求时都会发送的单个说明文件。文件中应包含项目级信息，即大多数聊天或 CLI 请求都适用的上下文，例如所用技术栈、项目概览、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建的说明文件。可针对特定语言（如 TypeScript 或 Astro）或任务（如创建 UI 组件或一组单元测试）提供指南。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
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

## 添加新的存储库标准

接下来只需进行一项编辑：添加项目级规则，要求使用 TSDoc 文档注释和文件级注释头记录代码。云代理稍后在本环境中处理分配给它的问题时，会读取这些规则。

编辑前，先创建工作分支（云代理会从问题目标分支读取说明，但仍建议将编辑单独提交）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并说明更新

云代理会从问题目标分支读取说明。在下一练习中将问题分配给 Copilot 时，它会从 `main` 分支创建分支。因此，说明更改必须先合并到 `main`，云代理才能读取。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求，并合并。这样，云代理在下一练习中处理问题时就能读取这些说明。

> [!TIP]
> 如果想在本工作坊中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。创建分支是为了让工作坊流程更贴近真实项目中的做法。

## 总结和后续步骤

你已了解 Copilot 如何从此项目的说明文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了存储库级标准。后续练习中，你会在云代理生成的拉取请求中看到这些标准的影响。

接下来，将你的第一个问题[分配给 Copilot 云代理][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [编写有效自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
