---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 检查工作

在本实验中，你使用 GitHub Copilot 完成了多项改善用户体验和添加功能的任务。你让 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，并实现包含高对比度和浅色模式开关的无障碍功能。现在检查代码更改；如有必要，向 Copilot 提供反馈以改进成果。

### 场景

如前所述，引入生成式人工智能（AI）并不会改变软件设计和开发运维（DevOps）的基本原则。我们始终需要检查生成的代码，并遵循常规 DevOps 流程。接下来，在开启团队其他成员的审查之前，先检查 GitHub Copilot 对文档、相关游戏功能和无障碍功能的建议。

## 安全和 GitHub Copilot 云代理

由于 Copilot 云代理异步且无人监督地执行任务，因此设置了某些安全限制，以确保安全性。这些限制包括：

- Copilot 仅对存储库具有读取权限，对其用于编写代码的分支具有写入权限。
- 云代理在 GitHub Actions 中运行，并会创建独立的临时环境来完成工作。
- 任何 GitHub Actions 工作流都必须先经人工批准才能运行。
- [默认情况下，外部资源访问受限](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，模型上下文协议（MCP）服务器也包括在内。

## 检查生成的文档

先检查 GitHub Copilot 云代理生成的第一个拉取请求（PR），也就是为代码添加文档。你将使用 GitHub.com 上的标准 PR 界面完成此任务。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括外部 MCP 服务器调用。你可以选择[自定义或禁用 Copilot 云代理的防火墙][agent-firewall]。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 或更完善的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 会包含 **[WIP]** 标记。若出现此情况，请等待 Copilot 完成工作。这可能需要几分钟；你可以趁此休息一下，或回顾到目前为止学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并检查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包含新添加的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自练习 2 开始时查看的自定义说明文件。

6. 检查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 你应能看到有工作流正在等待批准的指示器。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会在拉取请求的检查部分排队运行。如果一切正常，单个 Astro 应用的项目检查会全部通过。这可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向流程。你可以像提及团队其他成员一样，在拉取请求评论或代码内联评论中提及 Copilot。Copilot 会读取这些评论并启动新会话来处理请求。由于结果具有非确定性，我们无法提供固定的请求文本。你可以让 Copilot 更新以下内容：

- 在每个代码文件顶部添加简短说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，描述 Astro 应用结构。

1. 添加评论请求 Copilot 修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以使用上述想法，也可以根据你希望代码库包含的文档提出其他建议。
2. 选择 **View Session**，观察 Copilot 开始新会话并进行更新。
3. 你可以选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡检查更改。

可以继续迭代，直到满意为止。确认结果符合要求后，可以将 PR 从草稿转换为待审状态，然后合并到主分支。

![将 PR 转为待审状态](../../../_images/ex7-ready-for-review.png)

## 检查相关游戏功能

现在返回 Copilot 为解决“在游戏详情页显示相关游戏”问题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或更完善的 PR。
4. 选择 **Files changed** 选项卡，检查生成的代码。
5. 检查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 你应能看到有工作流正在等待批准的指示器。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会在拉取请求的检查部分排队运行。如果一切正常，单个 Astro 应用的项目检查会全部通过。这可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace，启动终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已实现相关游戏功能！和之前一样，你可以通过迭代让 Copilot 云代理更新功能。例如，可以要求 Copilot 调整显示的相关游戏数量，或确保添加注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义说明！）。和之前一样，可以在 **Conversation** 选项卡中添加评论，Copilot 会读取评论并启动新会话。

## 检查无障碍功能

最后，检查通过无障碍自定义代理实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在 Mission Control 请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或更完善的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会包含 **[WIP]** 标记。若出现此情况，请等待 Copilot 完成工作。这可能需要几分钟。

4. 选择 **Files changed** 选项卡，检查生成的代码。
5. 检查实现，尤其留意：
   - 用于切换模式的 UI 组件。
   - 使用本地存储持久保存用户偏好设置。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍富互联网应用（ARIA）标签、键盘导航等无障碍属性。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 检查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 你应能看到有工作流正在等待批准的指示器。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会在拉取请求的检查部分排队运行。如果一切正常，单个 Astro 应用的项目检查会全部通过。这可能需要几分钟。
10. **可选：**你可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace，启动终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式开关，确认功能正常且在重新加载页面后仍然生效。

留意无障碍自定义代理如何帮助 Copilot 遵循无障碍最佳实践实现这些功能。如果发现任何无障碍问题或改进空间，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 并请求更新。

## 可选练习 — 继续委派任务

云代理最适合处理真实待办事项，让你可以转而完成其他工作。可以尝试为存储库提交几个问题并分配给 Copilot，以培养这种工作习惯。例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云代理环境的全部课程。你：

- **检查了此存储库自带的自定义说明文件**，以便稍后观察这些说明对云代理输出的影响。
- **将问题分配给 Copilot 云代理**，并观察它设置环境、规划和异步执行任务。
- **创建并使用了自定义代理**来实现无障碍功能，添加高对比度和浅色模式开关。
- **使用 Copilot agents 页面作为 Mission Control**，监视并引导无障碍会话。
- **检查并迭代了云代理的拉取请求**，通过标记 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云代理环境的全部课程。如果想继续探索，其他环境课程可以补充你在这里练习的内容：

- 🖥️ **[VS Code 环境](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)** — 在终端中交付两个经过检查的更改，使用 Plan 和 Autopilot 模式、代理技能、QA 自定义代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云代理桥接。

你也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率问题分配给云代理。
- 为其他领域创建自定义代理。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [了解 Copilot 代理][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot 云代理设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云代理防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
