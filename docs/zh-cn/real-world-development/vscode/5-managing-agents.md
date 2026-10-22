---
title: "练习 5 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义代理][previous-lesson] |
|:--|

使用代理模式时，GitHub Copilot 会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作会在你的计算机上实时进行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以内联审阅每项差异、接受或拒绝单项更改，并通过后续提示引导对话，在不离开编辑器的情况下完善或扩展工作。

本练习将：

- 实时监控无障碍代理的工作。
- 审阅并接受建议的文件编辑。
- 在会话进行中引导代理，添加高对比度之外的浅色模式切换功能。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义代理正在实现高对比度模式。工作进行期间，设计团队提出了新需求：还希望为觉得默认主题太暗的用户添加浅色模式切换。无需从头开始新对话，你将引导现有会话扩展工作范围。

## 监控 Copilot 进度

Copilot Chat 视图会显示代理执行的每一步，包括它探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内联差异形式显示，并提供 **Keep** / **Undo** 控件。

1. 返回练习 4 中的 Copilot Chat 视图。
2. 代理工作时滚动聊天内容，查看每次工具调用。你会看到文件读取、文件编辑以及代理请求运行的终端命令。
3. 代理提出文件编辑时，差异会以内联形式显示：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 恢复更改，让代理重新尝试。
4. 也可以打开 Chat 顶部的 **Files changed** 指示器，查看代理在本次会话中修改的所有文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），代理会暂停并显示 **Allow** 提示。只批准你信任的命令。

5. 等待代理完成高对比度模式的实现。完成后应该会看到总结所做更改的最终消息。

## 引导代理添加浅色模式

高对比度模式就绪后，在同一对话中添加浅色模式切换。继续使用同一聊天会话时，代理会保留之前工作的所有上下文，包括修改过的组件、样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 代理会继续之前的工作，探索已有更改并提出浅色模式的新增更改。
3. 像审阅高对比度模式一样，逐项检查新更改，并使用 **Keep** 或 **Undo** 控件处理每个差异。
4. 如果代理的方向不符合预期，例如尝试以不喜欢的方式重构开关 UI，可以选择聊天输入框旁的 **Stop** 按钮停止操作，然后发送澄清后的后续消息。

> [!TIP]
> 引导正在进行的会话比从头开始更快。可以通过后续提示要求调整（“简化 CSS——使用 CSS 变量，而不是重复选择器”）、纠正方向（“不要添加第三个开关，将浅色模式整合到现有设置面板中”）或扩展范围（“为两个开关添加键盘快捷键”）。

## 审阅工作集

提交工作之前，快速检查代理修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审阅所有已更改文件。应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开若干已更改文件并检查差异。确认自定义代理中的无障碍实践已落实，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认无误后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你已使用 Copilot Chat 代理模式监控并引导正在进行的会话：审阅逐步生成的文件编辑、接受或拒绝更改，以及通过后续提示扩展工作而不丢失上下文。

本练习中，你了解了：

- 实时监控代理工作。
- 接受和拒绝单项文件编辑。
- 使用后续提示在会话进行中引导代理。

在[下一课][next-lesson]中，将无障碍功能发布为拉取请求，并审阅此实验中本地自定义代理生成的更改。

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
