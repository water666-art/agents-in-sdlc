---
title: "练习 3 - 使用 Playwright 模型上下文协议（MCP）服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚刚使用代理模式构建了筛选功能。创建拉取请求前，应确认它确实能在浏览器中正常工作。你不必自己逐个操作应用，而是连接 **Playwright MCP 服务器**，让 Copilot 操控真实浏览器测试功能，然后发布分支并创建拉取请求（PR）。

在本练习中，你将：

- 了解模型上下文协议（MCP）以及 MCP 服务器如何为 Copilot 扩展新工具。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用该服务器，在浏览器中手动测试筛选功能。
- 发布分支并为筛选工作创建拉取请求。

## 什么是模型上下文协议（MCP）？

当代理模式可以访问编辑器以外的资源时，功能会强大得多。Copilot 通过模型上下文协议（MCP）实现这一点：MCP 是代理与外部工具和服务通信的标准方式。

![显示代理模式内部工作原理及其如何与上下文、大型语言模型（LLM）和工具交互的示意图，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为人工智能（AI）代理提供了与外部工具和服务通信的方式。借助 MCP，AI 代理可以实时访问外部工具和服务，通过资源获取最新信息，并使用工具代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 代理与外部工具和服务之间的桥梁，并负责管理 AI 代理与外部工具（如现有应用程序编程接口（API）或 npm 包等本地工具）之间的通信。每个 MCP 服务器都代表一组 AI 代理可以访问的工具和资源。

一些常用的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API。AI 代理可以使用它创建新存储库、更新现有存储库，以及管理问题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能。AI 代理可以使用它浏览网页、填写表单和选择按钮等。

还有许多其他 MCP 服务器提供不同的工具和资源。GitHub 托管了一个 [MCP 注册表](https://github.com/mcp)，以便用户查找并为生态系统做出贡献。

> [!CAUTION]
> 出于安全考虑，应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请仔细检查源代码、验证发布者，并考虑安全影响。只使用可信的 MCP 服务器，并谨慎授予访问敏感资源或执行敏感操作的权限。

## 检查 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你应能看到已配置的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

此 `github` 条目随项目模板提供，因此 Copilot 能在上一练习中读取待办问题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装。VS Code 会通过 HTTP 连接服务器，并在 Copilot 首次使用其工具时让你通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控制的浏览器，正好可以用来测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 条目，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志会让 Playwright 在没有可见窗口的情况下运行浏览器。这是 Codespace 中的必需设置，因为 Codespace 没有可显示浏览器的桌面环境。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，因此通常已经安装了 Playwright 所需的浏览器。如果 Copilot 后续报告缺少浏览器，让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器，并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会显示 **Start** 操作。选择它立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过，VS Code 会在下一步中 Copilot 首次需要其工具时启动服务器，并询问你是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可为 Copilot 提供真实浏览器。你无需自己逐项操作应用来检查工作，代理可以打开页面、导航、应用筛选并读取结果，然后向你总结观察到的情况。这是无需离开对话、确认功能符合预期的最快方法。

在底层，Playwright MCP 服务器会使用页面的[无障碍树][playwright-mcp-server]，而不是屏幕截图。也就是说，代理会根据带标签的结构化元素（按钮、链接、列表项）进行推理，这与辅助技术的工作方式类似。因此，快速的功能检查也能顺便验证基本无障碍情况。

连接服务器并运行应用后，让 Copilot 测试刚刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐步完成操作并报告发现。根据问题中的验收条件检查总结。如果结果不符合预期，请提出后续问题，或让 Copilot 修复代码后再创建拉取请求。

> [!NOTE]
> 测试时，应用必须在 `http://localhost:4321` 上运行。如果开发服务器已停止，请先重新启动再发送提示。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器。如果它报告缺少浏览器，让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR）供团队检查。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板，并检查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 更改的 Source Control 面板屏幕截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

创建拉取请求有多种方式，例如使用 github.com 或 GitHub 命令行界面（CLI）。既然已经在使用 GitHub Copilot，就让它来创建拉取请求（PR）。它可以找到相关问题，并创建与问题关联的 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认代理下拉列表中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据提示选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会提示你通过 GitHub 身份验证；请按照提示操作。
5. 留意 Copilot 如何搜索问题、找到相关问题并创建 PR。
6. 选择 Copilot 生成的链接检查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！在本练习中，你：

- 了解了模型上下文协议（MCP）以及 MCP 服务器如何为 Copilot 扩展新工具。
- 将 Playwright MCP 服务器添加到了工作区。
- 使用该服务器在浏览器中手动测试了筛选功能，然后发布功能。
- 发布了分支并为筛选工作创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义代理][next-lesson]，以简化无障碍检查等专注任务。

## 资源

- [什么是 MCP，为什么大家都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
