---
title: "练习 3 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]可让你创建专门用于开发工作流中特定任务或领域的人工智能（AI）助手。通过在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义代理，你可以为 Copilot 提供明确的说明、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识编写为可复用的代理，例如负责执行[《网页内容无障碍指南》（WCAG）][wcag]合规检查的无障碍代理、遵循安全编码实践的安全代理，或统一测试模式的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局配置在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少要有 `name` 和 `description`，后面则是用于定义代理行为、专业知识和说明的 Markdown 提示。

### 自定义代理与代理技能的区别

自定义代理和[代理技能][agent-skills-concept]之间存在一些逻辑上的重叠。两者主要通过 Markdown 文件定义，并指导 AI 如何执行操作。最简单的区分方式是：**自定义代理是执行工作的角色，技能是工具**。

自定义代理拥有自己的上下文窗口，并且可以在工作过程中协调技能乃至其他代理。在本实验中，无障碍自定义代理会根据无障碍指南检查并更新网站；在工作过程中，它可以调用拉取请求工作流技能或运行并管理测试的技能。

> [!NOTE]
> 自定义代理没有唯一正确的编写方式。和 AI 的其他用法一样，应通过测试和迭代找到适合你的环境和场景的方案。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

使用自定义代理时，你将了解：

- 如何定义自定义代理。
- 如何将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，由于文本与背景颜色之间的对比度不足，有些用户难以阅读当前的深色主题。为解决这一无障碍问题，设计团队要求实现可由用户开关的高对比度模式。

无障碍功能至关重要，因此你希望尽快实现它。接下来使用自定义代理生成此功能。

## 检查无障碍自定义代理

已为无障碍工作创建了自定义代理。先检查其内容，了解它将如何引导 Copilot。

返回 Codespace，然后检查无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 接着浏览并检查以下部分：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉列表中显示 `.github/agents` 中定义的所有自定义代理。选择自定义代理后，该聊天会话便会使用相应代理的说明和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。不同练习会使用不同模式和模型，相关步骤会在需要时说明。

1. 如果代理下拉列表中尚未选中 **Agent**，请先选择它。

   ![显示 Chat 视图中代理选择器的屏幕截图](../../../_images/shared-chat-mode-selector.png)

2. 选择聊天视图底部的代理下拉列表（其中显示当前活动代理，默认为 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改并编辑项目文件。每项编辑都会在聊天中显示文件路径和差异，供你检查。
6. 随着编辑进行，**Files changed** 指示器会更新，以显示代理修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库，例如编辑 Astro 组件、CSS 和相关测试。

下一课中，你将检查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 中的自定义代理][custom-agents]，它们是针对特定任务和领域的专门 AI 助手。你可以使用自定义代理，将团队的专业知识和标准编写为可复用代理，帮助 Copilot 更有效地完成特定类型的工作。

你了解了以下概念：

- 如何定义自定义代理。
- 如何在 Copilot Chat 的代理模式中使用自定义代理。

接下来，你将[监视并引导代理的工作][next-lesson]，检查不断产生的更改，并在同一会话中添加浅色模式开关。

## 资源

- [了解自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监视和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
