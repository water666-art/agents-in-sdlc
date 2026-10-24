---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可在云端异步运行 GitHub Copilot。你在 GitHub 上分配任务后，云代理会在后台处理：探索存储库、进行更改并创建拉取请求，你可以同时处理其他工作。

在这些练习中，你将添加云代理需要遵循的自定义说明，然后分配 GitHub 问题并让它实现任务。你还将评审并使用自定义代理、在代理仪表板中监控和引导会话，最后评审云代理创建的拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加云代理将遵循的自定义说明 |
| [2. 云代理][ex2] | 异步代理 | 将问题分配给 Copilot 云代理 |
| [3. 自定义代理][ex3] | 专用代理 | 评审并使用自定义代理 |
| [4. 管理代理][ex4] | 监控 | 监控并引导代理会话 |
| [5. 迭代改进][ex5] | 评审 | 评审拉取请求、迭代改进 Copilot 的工作，并选择后续步骤 |

## 先决条件

参加本工作坊前，请确认你具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 已为帐户或组织启用 Copilot 云代理

> [!TIP]
> 没有付费计划？通过 [GitHub Education][callout-student-plan-education] 验证的学生可以免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊使用的代理、MCP、代码评审和 Copilot CLI 功能，因此可用于完成所有开发环境中的练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> 云代理适用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise**，不包括 Copilot Free。使用 Copilot Business 或 Enterprise 时，组织管理员必须为组织启用云代理。

> [!NOTE]
> 本开发环境不介绍模型上下文协议（MCP）。若要了解如何在 Copilot 中使用 MCP 服务器，请参阅 [CLI 开发环境](../cli/)或 [VS Code 开发环境](../vscode/)。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
