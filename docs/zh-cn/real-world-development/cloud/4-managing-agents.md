---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几节练习中，你要求 Copilot cloud agent 承担了三项改善用户体验和添加功能的任务。Cloud agent 能够异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub 上的[代理页面][agents-page]。在这个任务控制中心中，可以查看所有具有未关闭拉取请求（PR）的代理任务、了解执行的操作，还可以引导正在进行的会话。

本练习将：

- 浏览代理页面，监控 cloud agent 任务。
- 引导正在进行的会话，请求添加额外功能。

## 场景

将高对比度模式任务分配给代理后，团队意识到还可以同时添加浅色模式。既然网站样式和切换功能都在更新，加入这项功能也很合理。现在引导代理，确保它同时实现浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看分配给 Copilot cloud agent 的所有任务的当前状态。

1. 前往 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应该会看到分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的各项操作。

> [!NOTE]
> 具体列出的步骤会因 Copilot 的工作进度和采用的方案而异。

4. 同时留意右侧显示的拉取请求（PR）窗格。你可以在此查看 PR 和更改的文件，以便进一步监控。

## 引导 cloud agent

了解当前正在运行的任务后，请求 Copilot 在实现高对比度模式时一并添加浅色模式切换功能。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到显示设置已完成并开始处理任务。出现类似以下消息时，即表示任务已开始。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面中高亮显示“Steer active session while Copilot is working”对话框的任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 执行任务

与之前一样，Copilot 会开始处理更新后的任务！它会在完成收到消息时正在执行的步骤后，将新请求纳入工作流程。

和之前一样，这需要几分钟。可以趁此机会暂停一下，回顾目前为止学习和探索的内容。

## 总结和后续步骤

本练习介绍了 Copilot 代理页面，它是监控和引导 GitHub Copilot cloud agent 任务的中心。你可以使用任务控制中心跟踪所有正在进行和已完成的任务、审阅执行的工作，以及重定向正在进行的任务以调整范围或提供额外指导。

你了解了以下概念：

- 使用任务控制中心和代理页面监控 cloud agent 任务。
- 重定向正在进行的会话，以请求添加额外功能。

Copilot 正在完成无障碍功能。接下来，[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
