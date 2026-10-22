---
title: "练习 9 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云端委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。它们可以访问高级功能、后台信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在来探索其他实用命令。

在本练习中，将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 探索可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树和委派给 cloud agent 的可选命令。

## 场景

核心 CLI 工作流已经完成。现在看看其他功能：管理上下文、切换模型、共享会话，以及将工作（可选）委派给 [Copilot cloud agent][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理较大或复杂的任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；你也可以使用斜杠命令自行检查或压缩。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，请从存储库根目录启动它。
2. 输入：

   ```plaintext
   /context
   ```

3. 留意模型、当前 token 使用量，以及上下文如何分配给系统说明、工具、消息和剩余空间。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，结果可能不会有明显变化。

> [!NOTE]
> Copilot CLI 会在上下文窗口即将填满时自动压缩上下文。想自行决定压缩时间时，可以使用 `/compact`。切换到无关任务并希望开始全新对话时，使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有所长，开发者的偏好也各不相同。Copilot CLI 可以列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型和使用信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作和分享经验有助于提升每个人使用 AI 工具的能力。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看所安装版本中的 `/share` 选项。
2. 如果要共享此会话，请选择合适的目标位置，例如使用 `/share file` 导出本地 Markdown 文件。
3. 在发送给他人或发布之前，先检查导出的内容。会话导出内容可能包含提示、回复和项目详细信息。

发布链接或 gist 是可选操作。不要发布团队无意公开的存储库或对话内容。

## 可选：扩展工作或委派任务

核心工作坊内容已完成。Copilot CLI 还提供了用于处理较大任务的命令：

- `/fleet` 可将相互独立的子任务分配给多个子代理并行执行。
- `/worktree` 可为单独任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给 Copilot cloud agent，由它异步工作并可能打开拉取请求。

这些命令是可选的，因为它们可能创建额外的工作树或远程工作。尝试前，请先开始一个范围明确的新任务，并按照常规工作流审阅结果。如果要深入了解异步代理工作，请继续学习[云端代理工作坊][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以进行配置、共享会话并查看后台运行情况。在本练习中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 探索了可用模型。
- 了解了 `/share` 如何导出或共享会话。
- 探索了用于并行工作、工作树和委派给 cloud agent 的可选命令。

Copilot CLI 还有许多其他斜杠命令值得探索！最后，[回顾所学内容][next-lesson]并了解后续学习资源。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
