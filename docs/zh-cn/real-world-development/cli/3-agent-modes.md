---
title: "第 3 课 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案，使用 Autopilot 根据问题实现筛选功能，并使用 Interactive 模式检查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先前为项目添加了一项小功能，但较大的改动需要更严谨的流程。GitHub Copilot CLI 能融入组织现有的工作流，帮助我们以正确的方式构建正确的功能。本课开启了后续几课的代理驱动开发流程：从问题出发实现新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 后续功能开发会沿用同一个对话和分支。通常，不同类型的文件会在不同分支或 PR 中处理；这里为了专注于核心概念，会简化流程。

本课将带你：

- 从 GitHub 问题启动新的 Copilot 对话。
- 在 Plan 模式中定义需求。
- 使用 Autopilot 模式实现新功能。
- 检查代码。
- 在转发的浏览器中手动验证功能。

继续开发此功能时，你还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理，并打开功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩大，访客需要按类别和发行商筛选游戏。待办问题描述了此功能，但例如如何组合类别筛选等细节需要在编码前达成一致。你将使用 Plan 模式确定这些决定，然后授权 Autopilot 在限定范围内实现。

## 背景

将人工智能（AI）编码代理引入开发流程并不会改变软件开发的基本原则。事实上，这些原则可能变得更加重要！大多数开发者遵循的流程大致如下：

1. 从描述待办事项的问题开始。
2. 制定要构建内容的计划。
3. 编写并检查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这种标准流程，可以确保 AI 生成的代码满足既定要求，并像手写代码一样经过审查。

## 对话模式

**对话模式**控制代理的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你和代理协同工作。代理提出更改，并等待你的输入后再继续。
- **Plan**：代理先创建计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作，包括编写代码、运行测试并反复迭代，直到任务完成。

先在 Plan 模式中开始并检查计划，然后切换到 Autopilot 模式进行实现。

## 从问题开始

开始筛选功能前，返回 Codespace 并确保存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重启。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有问题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的问题。
9. 按 <kbd>C</kbd> 将问题添加到提示中，并返回 **Session** 选项卡。

请注意，提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的问题或拉取请求（PR）添加到上下文中。

## 规划筛选功能

规划可以让你在交由 Copilot 执行任务之前，确定实现功能或完成任务的方法。处理复杂任务时，先花些时间规划总是有益的。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的问题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划：先探索项目，再确定最佳方案。

3. Copilot 可能会询问筛选功能应如何工作。根据你的偏好回答即可，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动检查计划。
6. 如果计划的任何部分与已确定的决定不符，请让 Copilot 修改。

## 批准 Autopilot

计划已经编写并检查完毕，现在开始实现。使用 Autopilot 让 Copilot 执行工作。

Autopilot 会让 Copilot 持续迭代，直到它认为问题已解决。

1. 选择 **Accept plan and build on autopilot (recommended)**，或你所安装版本中含义相近的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按照已确定的计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后，实现可能会立即开始，因此请先检查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再将检查视为已完成。

## 检查并验证实现

代码生成后，应像其他代码一样在合并前进行检查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 将结果与问题及规划时确定的决定进行比较。
4. 检查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 检查项目运行的结果，并让 Copilot 修复失败项。

## 体验新功能

代码看起来没问题，但实际能否运行？像之前一样启动应用，并通过 Codespaces 转发的端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及按计划中确定的条件组合筛选。
4. 确认重置和空结果行为符合问题描述及已确定的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同的对话模式构建并检查功能。在本课中，你：

- 从 GitHub 问题启动了新的 Copilot 对话。
- 在 Plan 模式中定义了需求。
- 使用 Autopilot 模式实现了新功能。
- 检查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，我们将[使用自定义说明][next-lesson]进一步了解如何生成符合已记录实践的代码。

## 资源

- [在 GitHub Copilot CLI 中使用 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
