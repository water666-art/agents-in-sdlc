---
title: "练习 4 - 监视和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云代理处理了三个独立任务，旨在改善用户体验并添加功能。云代理虽然可以异步自主运行，但监视这些任务仍然很重要。

你可以使用多种工具管理分配给云代理的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个控制中心中，你可以查看所有带有未关闭拉取请求（PR）的代理任务、探索已执行的操作，甚至引导正在进行的会话。

在本课中，你将：

- 了解代理页面，并监视云代理任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将高对比度模式任务分配给代理后，团队意识到现在也适合添加浅色模式。网站样式和切换功能都已在更新，因此将此功能一并实现很合理。现在需要引导代理，同时添加浅色模式和高对比度模式。

## 检查 Copilot 云代理任务

查看当前分配给 Copilot 云代理的所有任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应能看到分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的应用程序编程接口（API）。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，检查 Copilot 已执行的操作，包括：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置模型上下文协议（MCP）服务器。
    - 执行完成已分配任务所需的各种步骤。

> [!NOTE]
> 列出的具体步骤会因 Copilot 当前的工作状态和所采用的方法而异。

4. 同时留意右侧显示的拉取请求（PR）窗格。你可以在此查看 PR 和已更改文件，以便进一步监视。

## 引导云代理

现在你已了解活动任务，请求 Copilot 在处理高对比度模式时也加入浅色模式开关。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![Mission Control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示已完成设置并开始工作。看到类似以下消息时，即表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面上突出显示 Steering active session while Copilot is working 对话框的云代理任务屏幕截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！它会先完成收到消息时正在执行的步骤，然后将新请求纳入工作流程。

同样，这需要几分钟，正好可以暂停一下，回顾目前学到和体验的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，它是监视和引导 GitHub Copilot 云代理任务的中心位置。使用此控制中心，可以跟踪所有活动和已完成任务、查看正在执行的工作，甚至重定向正在进行的任务以调整范围或补充指导。

你了解了以下概念：

- 使用 Mission Control 和代理页面监视云代理任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来，你将[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [了解自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
