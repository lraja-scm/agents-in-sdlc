---
title: "练习 3 - 自定义代理"
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

通过自定义代理，你将了解：

- 如何定义自定义代理。
- 如何向自定义代理分配任务。

## 场景

Tailspin Toys 致力于确保所有用户都能访问其众筹平台，不受视觉能力或偏好影响。近期用户反馈指出，由于文字与背景颜色对比度不足，部分用户难以阅读当前的深色主题。为解决这一无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

无障碍功能至关重要，因此希望尽快实现。你将使用自定义代理生成此功能。

## 检查无障碍自定义代理

我们已为你创建了一个无障碍自定义代理。先检查它的内容，了解它会如何引导 Copilot。

返回 Codespace，然后检查无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> `name` 和 `description` frontmatter 是自定义代理的必填内容。

3. 浏览并检查后续各节，其中介绍：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误。

## 创建并分配议题

任务控制中心是管理环境中所有代理的中心位置。你可以在此将任务分配给 Copilot cloud agent、监控任务，或重定向并提供额外指导。现在将创建高对比度模式任务并分配给 Copilot。

1. 前往存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设为 `Add high contrast mode to website`。
6. 将 **Description** 设为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

    ![云端代理分配截图，其中突出显示了自定义代理和无障碍选项](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务！

## 总结和后续步骤

本练习介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，它们是针对特定任务和领域定制的专用 AI 助手。使用自定义代理可以将团队专业知识和标准编码为可复用代理，从而更有效地引导 Copilot 完成特定类型的工作。

你了解了以下概念：

- 如何定义自定义代理。
- 如何向自定义代理分配任务。

Copilot 正在实现高对比度模式。现在可以前往[任务控制中心监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [在组织中使用自定义代理的准备工作][org-custom-agents]
- [在企业中使用自定义代理的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
