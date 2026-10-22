---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]让你可以针对开发工作流中的特定任务或领域创建专用 AI 助手。通过在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义代理，可以为 Copilot 提供专注的说明、最佳实践、编码模式和领域知识，引导它更有效地完成特定类型的工作。团队可以将专业经验编码为可复用代理，例如确保符合 [WCAG][wcag] 标准的无障碍代理、遵循安全编码实践的安全代理，或维护一致测试模式的测试代理。

自定义代理使用项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少包括 `name` 和 `description`，后面跟着定义代理行为、专业知识和指令的 Markdown 提示。

### 自定义代理与代理技能的比较

自定义代理和[代理技能][agent-skills-concept]在逻辑上有一些重叠。两者主要都通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理**是工作者，**技能**是工具。

自定义代理拥有自己的上下文窗口，并且能够协调技能（甚至其他代理）完成工作。在本实验中，无障碍自定义代理会根据无障碍指南审阅并更新网站；在此过程中，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一的“正确”方式。和 AI 的其他用法一样，应针对环境和场景不断测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于确保所有用户都能访问其众筹平台，不受视觉能力或偏好影响。近期用户反馈指出，由于文字与背景颜色对比度不足，部分用户难以阅读当前的深色主题。为解决这一无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

无障碍功能至关重要，因此希望尽快实现。你将使用自定义代理生成此功能。

通过本练习，将：

- 检查现有的无障碍自定义代理。
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 检查无障碍自定义代理

我们已为你创建了一个无障碍自定义代理。先检查它的内容，了解它会如何引导 Copilot。

返回 Codespace，打开终端，并从 `main` 创建用于无障碍工作的新分支（筛选功能 PR 会与本次更改保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> `name` 和 `description` frontmatter 是自定义代理的必填内容。

3. 浏览并检查后续各节，其中介绍：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉列表中显示 `.github/agents` 中定义的所有自定义代理。选择自定义代理后，当前 Chat 会话会使用该代理的说明和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新对话。每项练习可能使用不同模式和模型，相关步骤会具体说明。

1. 如果 Chat 视图代理下拉列表中尚未选择 **Agent**，请先选择它。

   ![显示 Chat 视图中代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉列表（其中显示当前代理，默认是 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作，探索代码库、提出更改并修改项目文件。每项编辑都会以内联形式显示在聊天中，包括文件路径和供你审阅的差异。
6. 代理修改文件时，**Files changed** 指示器会更新，以显示本次会话中已修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库中的文件，例如 Astro 组件、CSS 和相关测试。

下一练习将检查并引导这项进行中的工作。

## 总结和后续步骤

本练习介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，它们是针对特定任务和领域定制的专用 AI 助手。使用自定义代理可以将团队专业知识和标准编码为可复用代理，从而更有效地引导 Copilot 完成特定类型的工作。

你了解了以下概念：

- 如何定义自定义代理。
- 如何在 Copilot Chat 代理模式中使用自定义代理。

接下来，[监控并引导代理的工作][next-lesson]，审阅正在发生的更改，并在同一会话中添加浅色模式切换功能。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 中的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
