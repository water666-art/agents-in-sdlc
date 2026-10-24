---
title: "练习 2 - GitHub Copilot 云代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都面临技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云代理能够自主执行更新代码和添加功能等任务。完成后，它会生成草稿拉取请求（PR），供开发者检查。这样可以委派繁琐任务、加快开发流程，并让开发者专注于更重要的工作。

你将通过 Copilot 云代理了解：

- 如何自定义代码生成环境。
- 如何确保操作安全。
- 明确界定问题范围的重要性。
- 如何将问题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受雇开发网站的承包商没有留下理想的文档，实际上完全没有文档。首先，团队希望为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望在每款游戏的详情页显示相关游戏，也就是同一类别中的其他游戏，让用户可以继续浏览。团队暂时不需要经过精心打磨的实现，只希望有可用于用户体验验收测试的版本。这项工作目前受阻，但还有其他优先级更高的问题。

这两类任务都可能被推迟处理，非常适合分配给 Copilot 云代理。云代理可以异步处理任务，让开发者专注于其他工作，然后再回来检查 Copilot 的成果并确认符合预期。

## 认识 GitHub Copilot 云代理

[GitHub Copilot 云代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像人类开发者一样在后台执行任务。与人类开发者协作类似，你可以通过多种方式交付任务，包括[将 GitHub 问题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配问题后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。会话进行中或完成后，你都可以查看详情。方案准备好供你检查时，Copilot 会在拉取请求中通知你。

## 明确界定任务范围的重要性

GitHub Copilot 并没有什么魔法，使用它并不意味着可以跳过思考。即使看起来很简单的操作，深入分析后也可能十分复杂，因此[为 Copilot 云代理明确界定任务范围][cloud-agent-best-practices]非常重要。应像对待人工智能（AI）结对编程伙伴一样对待它：分阶段工作、学习、尝试并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义说明

前面的练习介绍过自定义说明及其如何引导 Copilot。如果你已经完成自定义说明的实操练习，就看过 `.github/copilot-instructions.md` 的作用；如果还没有，现在正好快速查看一下。在将任务分配给 Copilot 云代理之前，先只读查看此存储库中已有的说明文件，这样稍后就能观察到它们的影响。

可以在存储库的 GitHub 网页界面中打开以下文件，也可以在已运行的 Codespace 中查看：

- `.github/copilot-instructions.md` - 查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md` - 留意 `applyTo` frontmatter，它将说明的适用范围限定为 `**/*.test.ts`。

在下一节将 `Code lacks documentation` 问题分配给云代理时，留意生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自上述说明文件。稍后的练习中还会再次检查拉取请求。

## 为 Copilot 云代理设置开发环境

无论由谁编写代码，通常都需要特定环境，并运行一些设置脚本来确保环境状态良好。将任务分配给 Copilot 时也是如此，因为它会以类似软件工程师（SWE）的方式执行任务。

云代理使用 [GitHub Actions][github-actions]作为工作环境。你可以通过创建[专用设置工作流][setup-workflow]来自定义环境。工作流配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，并会在云代理开始工作前运行，以便它使用必要的开发工具和依赖项。该工作流已预先配置，以支持实验流程并提供学习机会。它会为 Copilot 安装 Node.js、项目依赖项和端到端测试所需的浏览器二进制文件，并为单个 Astro 应用迁移和填充 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许在存储库的“Actions”选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设为步骤所需的最低级别。Copilot 会获得自己的操作令牌。
    permissions:
      # 例如，如果要在设置步骤中克隆存储库并安装依赖项，需要 `contents: read` 权限。如果不在设置步骤中克隆存储库，Copilot 会在步骤完成后自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现已基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试使用其中的数据。
      - name: Set up the database
        run: npm run db:setup
```

这看起来与其他 GitHub 工作流文件类似，但有几个关键点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会先执行此作业。
- 注意 `workflow_dispatch` 触发器，它允许你从存储库的 **Actions** 选项卡手动运行工作流。这样可以在等待 Copilot 运行工作流之前，先测试它能否成功运行。

## 添加文档

大家都知道文档的重要性，但大多数项目要么信息过时，要么完全缺少文档。这类技术债务往往得不到处理，会降低工作效率，也增加维护代码库或帮助新开发者加入团队的难度。幸运的是，Copilot 擅长创建文档，因此这是分配给 Copilot 云代理的理想问题。它会在后台生成所需文档。后续练习中，你会回来检查生成的内容。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建问题对话框。
4. 选择 **Blank issue** 创建空白问题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给问题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），其标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方案。

13. 检查列表以及 Copilot 准备完成的任务。
14. 向下滚动拉取请求时间线，应该会看到 Copilot 已开始处理问题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 查看实时会话，观察 Copilot 如何解决问题，包括探索代码和了解当前状态、暂停以思考并确定适当计划，以及创建代码。

这可能需要几分钟。Copilot 云代理的主要目标之一是异步执行任务，让开发者可以专注于其他工作。我们会利用这一特性：先将另一项任务分配给 Copilot 云代理，然后转而为应用编写代码并添加功能。

## 在游戏详情页添加相关游戏区域

如前所述，GitHub Copilot 云代理的一大优势是可以分配工作，让你专注于一组任务，同时让代理处理另一组任务。为设计团队添加相关游戏区域或许不需要很长时间，但这些时间仍可用于其他工作。现在将它分配给 Copilot 云代理。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建问题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为：`Show related games on the game details page`
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给问题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，问题的第一条评论中应会出现一组 👀，表示 Copilot 正在处理任务！

![Copilot 使用眼睛表情符号表示正在处理问题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在认真处理第二个请求！Copilot 云代理的工作方式类似软件工程师，因此你无需持续监控，而是在完成后进行检查。现在来看看如何创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云代理][copilot-agents]。你可以将问题分配给云代理，让它异步处理任务。Copilot 可以协助处理技术债务、开发新功能，或将代码迁移到其他框架。

你了解了以下概念：

- 如何自定义代码生成环境。
- 如何确保操作安全。
- 明确界定问题范围的重要性。
- 如何将问题分配给 Copilot。

Copilot 云代理正在后台工作，现在可以转而学习如何创建和使用自定义代理。[Copilot 云代理也可以使用模型上下文协议（MCP）服务器][cloud-agent-mcp]，并且可以读取自定义说明；我们已在前面的模块中介绍过这些内容。

## 资源

- [了解 Copilot 云代理][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot 云代理设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
