---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "了解存储库说明，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，就应让 Copilot 能够获取相关指导。[说明文件][instruction-files]不仅描述你希望编写什么代码，也说明代码应如何组织。现在你已构建筛选功能，接下来将了解 Copilot 使用的说明、添加文档标准，并将其应用于代码。

在本课中，你将：

- 了解存储库说明和按路径限定范围的说明文件如何传递给代理。
- 更新说明文件，确保遵循编码标准。
- 查看说明文件对代码的影响。

## 场景

Tailspin Toys 与其他优秀开发团队一样，有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决定，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 记录其用途、参数和返回值；如存在可注入的 `db` 参数，也应加以说明。
- 可复用的 Astro 组件应记录其 `Props` 约定；相关代码更改时，注释也应保持更新。
- 应保留现有格式和 lint 指引。

通过说明文件，可以确保 Copilot 获取正确的信息，并按照这些实践完成任务。

## 说明文件

自定义说明可向 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。这项功能可以帮助你引导 Copilot 提供更符合需求的建议和代码片段。你可以指定偏好的编码约定、库，甚至代码注释的风格。你可以为整个存储库创建说明，也可以为特定文件类型提供任务级上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：每次向 Copilot 发出存储库请求时都会发送的单个说明文件。文件中应包含对大多数请求都适用的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指南的说明文件。

> [!NOTE]
> 其他说明格式及其支持情况因环境而异。使用特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 查看项目中的自定义说明文件

入门项目已包含一组说明文件。先了解现有内容，再进行修改，以便观察其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器中（而不是终端中）打开 `.github/copilot-instructions.md`。
3. 查看文件，留意项目简介和编码指南。该文件适用于此存储库中与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹并查看其中的文件。你会看到 Astro 文件、Drizzle 数据层、测试等方面的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了决定说明适用文件的 glob 模式。
6. 打开 `.github/instructions/drizzle.instructions.md`，留意其中指向其他说明文件和现有项目文件的链接。这样可以将较大的说明拆分成更小且可复用的文件，并提供示例供 Copilot 参考。

## 更新说明文件以符合团队指南

现有文件是良好的起点，但仍有不足。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中，找到大约位于第 35 行的 **Code formatting guidance** 部分。
2. 在该部分最后添加以下项目符号：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指南

Copilot CLI 会在对话开始时加载存储库说明。保存更新后的说明后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时使用新指南。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指南：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。留意新增的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何从说明文件获取上下文，并将新标准应用于功能。具体来说，你：

- 了解了存储库说明和按路径限定范围的说明文件如何传递给代理。
- 更新了说明文件，确保遵循编码标准。
- 查看了说明文件对代码的影响。

接下来，你将[自定义并运行可复用的 quality-checks 技能][next-lesson]，以确保 lint 和测试始终得到一致执行。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [编写有效自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
