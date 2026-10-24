---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发者，你通常需要查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件、再次修改……工作往往会不断增加。

这正是 Copilot 代理模式的用途。

Copilot 代理模式可以在集成开发环境（IDE）中更自主地工作。它的行为类似开发者：先探索现有项目结构，执行必要的更新，运行测试等任务，并自动修复发现的问题。现在来看看如何使用代理模式为网站添加新功能。

> [!NOTE]
> 虽然名称相似，但代理模式和云代理对应两种不同的体验。代理模式在集成开发环境（IDE）中执行任务，便于快速反馈和交互。云代理则像团队成员一样异步工作，通过问题和拉取请求与你协作。

在本练习中，你将了解：

- Copilot 代理模式如何探索项目、识别相关文件并协调进行更改。
- GitHub Copilot 代理模式如何跨用户界面（UI）和数据层实现新功能。
- 合并到代码库前，如何检查 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断扩大，你希望允许用户按类别和发行商筛选游戏。上一练习中已添加发行商辅助函数，现在使用 Copilot 代理模式完成其余的数据层、用户界面（UI）和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先探索 Tailspin Toys 网站，了解其当前功能。

此网站是一个带有开发者主题的桌面游戏众筹平台。用户可以列出游戏并查看游戏详情。整个网站是单个 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 frontmatter 使用 Drizzle ORM 直接查询本地 SQLite 数据库，不存在独立的后端应用程序编程接口（API）或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能通过使用标准 DOM API 的小型、局部 Astro `<script>` 添加。

### 启动网站

项目提供了 `npm run dev` 任务，可启动单个 Astro 开发服务器，简化网站运行过程。按照以下步骤，在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后，你应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，应会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭对话框。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中，从终端选择 localhost URL 链接会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的专用隧道，现在 Codespace 正在其中托管 Web 服务器。

### 探索网站

网站运行后，可以探索其功能。网站的主要功能包括：

- **主页**：显示桌面游戏列表及其标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，其中包含标题、说明、发行商和类别等更多信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。不同练习会使用不同模式和模型，相关步骤会在需要时说明。

网站的初始实现已可正常使用，但还可以添加新功能。先查看待办事项列表。根据模板设置项目时，系统已自动创建一组问题作为待办事项。让 GitHub Copilot 显示这些问题。

1. 在 Chat 视图的代理下拉列表中选择 **Agent**。**Agent** 代理可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示 Chat 视图中代理选择器的屏幕截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非表示偏好某个模型。构建本工作坊时使用了 Claude Sonnet 4.5，因此将其写入说明，希望代码建议相对一致，帮助你获得良好体验。不过，由于大型语言模型（LLM）的结果具有概率性，你可能会看到与工作坊示例不同的建议。这完全正常，属于预期情况。

> [!NOTE]
> 由于结果具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示，询问 GitHub 存储库中的待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行命令以列出所有问题。
5. 查看生成的问题列表。

你会发现 Copilot 还根据它认为对用户最有用的程度为问题确定了优先级。

## 检查说明文件

让代理生成代码前，先检查将用于向 Copilot 提供上下文的说明文件。接下来会使用[用户界面（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)说明文件，其中包含如何为网站添加功能的上下文。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于如何添加功能的总体指南，包括：
   - 架构概览。
   - 组件设计、可测试性和无障碍原则。
   - 指向特定文件类型说明的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置，因此可以复用说明、将复杂内容拆分成更易管理的部分，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行三项单独的更新：

- 在数据层（`src/lib/`）中添加或完善其余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，加入筛选 UI。

此外，合并前必须运行并通过测试。Copilot 代理模式可以为你完成这些任务，现在开始添加功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 说明文件。

   ![选择 UI 说明文件的屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉列表中仍选中 **Agent**。

   ![显示 Chat 视图中代理选择器的屏幕截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选中 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据待办事项中的相关问题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并定位功能相关文件。你应能看到它找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> 你会看到 Copilot 执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 选项，因此无需在工作进行时立即选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中询问是否允许运行测试的对话框屏幕截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至有些测试会失败。这没有问题！Copilot 会在代码生成和测试之间反复迭代，直到完成任务并且未检测到错误。

   ![包含完整 Copilot 代理模式会话的 Chat 屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，留意潜在问题。

> [!CAUTION]
> 务必检查 Copilot 或任何 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，探索新功能。
11. 确认一切正常并检查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习中，你使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加了新功能。你了解了：

- GitHub Copilot 代理模式如何跨用户界面（UI）和数据层实现新功能。
- Copilot 代理模式如何探索项目、识别相关文件并协调进行更改。
- 合并到代码库前，如何检查 Copilot 代理模式生成的更改和测试。

现在使用 [Playwright MCP 服务器测试功能][next-lesson]，然后为其创建拉取请求。

### 额外探索练习 - 实现分页

随着游戏列表不断扩大，需要启用分页功能。使用本练习中学到的技能，提示 Copilot 更新网站以实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用现有说明文件。
- 考虑分页方式，例如允许用户选择每页大小，或将其设置为固定值。
- 编写提示时提供必要的指导，确保 Copilot 按照你的期望实现功能。
- 你可能需要通过请求更改和补充上下文，与 GitHub Copilot 反复沟通。这是正常的 Copilot 使用流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：用途和适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
