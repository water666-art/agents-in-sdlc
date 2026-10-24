---
title: "第 7 课 - 创建并使用质量保证（QA）代理"
description: "创建质量保证（QA）自定义代理，将问题需求、quality-checks 技能和 Playwright 模型上下文协议（MCP）结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，并通过 Playwright 模型上下文协议（MCP）在浏览器中观察筛选功能。现在将这些能力整合到具有明确 QA 流程的自定义代理中。

在本课中，你将：

- 了解自定义代理如何与说明、技能和 MCP 工具协作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 代理，并根据筛选问题检查其发现。

## 场景

在创建拉取请求（PR）之前，Tailspin Toys 希望统一检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义代理可以协调 QA 流程，并生成可复用的报告。

## 什么是自定义代理？

自定义代理是在 Markdown 配置文件中定义的 Copilot 专用版本。配置文件描述代理的用途、说明和可用工具。本工作坊将通过 `.github/agents/qa.agent.md` 定义 QA 角色，并在 Copilot CLI 中选择该角色。

你使用过的自定义功能各有不同用途。存储库说明描述团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则告诉 Copilot 如何使用这些能力评估需求并报告发现。它不会取代其他功能，也不需要另开对话。

## 创建 QA 配置文件

打开功能 PR 前，让 Copilot 创建可复用的 QA 配置文件。该配置文件会定义 QA 执行的检查和必须遵循的边界。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 让默认代理创建新的自定义代理：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新代理前，先检查其配置文件，确认 Copilot 正确记录了预期的 QA 工作流和边界。这有助于避免代理在你只想验证功能时，进行不完整或范围过大的更改。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 是可选项，但填写后可为代理设置清晰的显示名称。
3. 阅读配置文件说明，并确认 QA 会从需求开始、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持性证据、在更改实现代码前征求同意，并且不会提交更改或打开拉取请求。
5. 如果生成的配置文件缺少上述职责或边界，请让默认代理修改后再继续。

## 根据问题运行 QA

Copilot CLI 会在对话开始时加载项目代理。创建配置文件后，恢复同一个筛选对话，再选择 QA，以便它能够使用已有的问题和规划决定。

1. 使用以下提示启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于代理刚刚创建，可能不会出现在可用代理列表中。它仍然可用，上述命令会启用它。

2. 使用以下提示让 QA 代理检查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 代理会开始工作。
4. 完成后，阅读它提供的报告。

## 总结和后续步骤

你已向工作流中添加可复用的专门角色，并检查了它的工作。在本课中，你：

- 了解了自定义代理如何与说明、技能和 MCP 工具协作。
- 创建并检查了可复用的质量保证配置文件。
- 选择了 QA 代理，并根据筛选问题检查了其发现。

现在实现、技能更新、QA 配置文件、测试和验证报告都已准备就绪。接下来，你将[整合这些内容，创建功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
