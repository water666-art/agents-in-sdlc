---
title: "练习 5 - 监视和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义代理][previous-lesson] |
|:--|

使用 GitHub Copilot 代理模式时，它会自主探索代码库、提出更改、编辑文件和运行命令。由于这些工作会实时在本地计算机上进行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以直接在聊天中查看每项差异、接受或拒绝单独更改，并通过后续提示引导对话，在不离开编辑器的情况下完善或扩展工作。

在本练习中，你将：

- 实时监视无障碍代理的工作。
- 检查并接受建议的文件编辑。
- 在会话中途引导代理，除了高对比度模式之外，还添加浅色模式开关。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义代理正在实现高对比度模式。工作进行期间，设计团队提出了另一个需求：还希望为觉得默认主题太暗的用户添加浅色模式开关。无需从头开始新对话，你将引导现有会话扩展工作范围。

## 监视 Copilot 的进度

Copilot Chat 视图会显示代理执行的每个步骤，包括它探索的文件、运行的命令和提出的编辑。每项文件编辑都会在聊天中内嵌显示差异和 **Keep** / **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 代理工作时滚动聊天记录，查看每次工具调用。你会看到文件读取、文件编辑和它请求运行的终端命令。
3. 代理提出文件编辑时，聊天中会显示差异：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 撤销更改，让代理重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看代理在本会话中修改的所有文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），代理会暂停并显示 **Allow** 提示。只批准你确认安全的命令。

5. 等待代理完成高对比度模式的实现。完成后应会看到总结更改的最终消息。

## 引导代理添加浅色模式

高对比度模式已就绪，现在扩展同一对话，添加浅色模式开关。由于继续使用同一个聊天会话，代理会保留之前工作的上下文，包括已修改的组件、采用的样式方法和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 代理会在先前工作的基础上，探索已有更改并提出浅色模式的新增内容。
3. 新编辑出现时，像检查高对比度模式一样逐项检查。对每项差异选择 **Keep** 或 **Undo**。
4. 如果代理采取了你不希望的方向，例如以不合意的方式重构切换 UI，可以使用聊天输入框旁边的 **Stop** 按钮停止它，然后发送澄清消息。

> [!TIP]
> 引导正在进行的会话比从头开始更快。可以使用后续提示请求调整（“简化 CSS，使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个开关，将浅色模式整合到现有设置面板中”）或扩展范围（“同时为两个开关添加键盘快捷键”）。

## 检查工作集

提交工作前，快速检查代理修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 检查完整的已更改文件列表。你应能看到 Astro 组件、样式和相关测试的更新。
3. 打开几个更改文件并查看差异。确认代码体现了自定义代理要求的无障碍模式，包括无障碍富互联网应用（ARIA）属性、键盘导航、语义化 HTML 和通过本地存储持久保存设置。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课中将发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 代理模式监视并引导正在进行的会话，逐项检查文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留原有上下文。

本课中你：

- 实时监视了代理的工作。
- 接受或拒绝了单独的文件编辑。
- 通过后续提示在会话中途引导代理。

在[下一课][next-lesson]中，你将把无障碍更改作为拉取请求发布，并检查本实验中本地自定义代理生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
