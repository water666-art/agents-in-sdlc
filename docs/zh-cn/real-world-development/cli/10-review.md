---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义配置和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装了 Copilot CLI、探索了项目，并找到了预置的筛选问题。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选问题开始，在 Plan 模式中确定方案，在 Autopilot 模式中实现功能，并在 Interactive 模式中检查代码。
- 使用自定义说明引导代理，然后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加了 Playwright 模型上下文协议（MCP）服务器，并通过真实浏览器体验筛选功能。
- 创建并选择质量保证（QA）自定义代理，以评估需求、覆盖率、技能结果和浏览器证据。
- 检查了完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索了用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 交付的内容

本工作坊有两个 PR 里程碑，每个里程碑都基于已更新的 `main` 分支创建了自己的分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选功能和质量工作流**：实现筛选功能、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并包含相关测试。

从规划筛选功能到打开 PR，你一直使用同一个对话和分支。为简化工作坊流程，这些工作被合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、自己在浏览器中检查，以及通过 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并以新格式报告结果。QA 将这些结果与需求和测试覆盖率检查结合起来，为 PR 提供评估。

新增测试应填补真实缺口；如果 QA 运行时无需新增测试，也可能是正确结果。授权合并前，请检查代码和证据；如果有更改，请重新获取受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响其工作。在本工作坊中，你更新了说明、自定义了技能、创建了 QA 配置文件，并配置了 MCP 服务器。可在多个对话中复用这些配置，并根据团队需求进行调整。说明设定标准，技能描述可重复执行的任务，自定义代理定义专门角色，MCP 服务器则连接外部工具。检查实际更改和工具结果，而不只是代理的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在实现前思考方案，使用 **Interactive** 参与专注的小幅更改，使用 **Autopilot** 处理范围明确的任务。日常修改可选择速度更快的模型，复杂工作则选择能力更强的模型。

上下文仍然和基础设施同样重要。清楚说明要构建的内容、原因和方式，会显著影响输出。

## 更多探索

你已完成核心工作流。还可以了解以下 CLI 功能：

- `/review`：请求代码审查代理分析更改。
- `/rubber-duck`：讨论问题并获得另一个视角。
- `/fleet`：协调并行执行的独立子任务。
- `/worktree`：隔离独立任务。
- `/delegate`：将任务发送给 Copilot 云代理。

## 后续步骤

熟练使用任何工具的最佳方式就是持续实践！可以将它用于生产代码、个人项目，或用于实现一个酝酿已久却一直没时间动手的小应用。与团队分享经验，也向团队成员学习。还可以继续探索相关文档。

如果想从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他环境，请探索 [VS Code 工作坊][vscode]、[GitHub Copilot 应用工作坊][app]或 [Copilot 云代理工作坊][cloud]。

## 资源

- [了解 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
