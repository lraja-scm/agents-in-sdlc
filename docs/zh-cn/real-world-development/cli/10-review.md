---
title: "练习 10 - 回顾和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的完整工作流中使用了 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选议题。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中构建，并在 Interactive 模式中审阅。
- 使用自定义说明引导代理，然后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加了 Playwright 模型上下文协议（MCP）服务器，并用它在真实浏览器中探索筛选功能。
- 创建并选择质量保证（QA）自定义代理，以评估需求、测试覆盖率、技能结果和浏览器证据。
- 审阅了完整的筛选功能更改，并为筛选 PR 授权使用 Agent Merge。
- 探索了用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 交付内容

本工作坊有两个 PR 里程碑，分别基于更新后的 `main` 创建自己的分支：

1. **星级评分**：在游戏卡片上显示已有的 `starRating`，并明确表示尚未评分的状态。
2. **筛选和质量工作流**：实现筛选功能、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并包含相关测试。

从规划筛选功能到打开 PR，你一直使用同一对话和分支。为了简化工作坊，我们将这些工作合并到同一个 PR 中。

## 不同类型的验证

你通过多种方式检查了代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 使用 Copilot 探索浏览器。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新的格式报告结果。QA 汇总这些结果，并在 PR 创建之前审阅需求和测试覆盖率。

添加的测试应弥补实际缺口；QA 运行后无需添加测试也可能是正确结果。授权合并前，请审阅代码和证据；更改后，应更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了说明、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。说明用于设定标准，技能用于描述可重复的任务，自定义代理定义专家角色，MCP 服务器连接外部工具。请审阅实际更改和工具结果，而不只是代理的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 先规划方案，使用 **Interactive** 跟进专注的小型更改，使用 **Autopilot** 处理范围明确的任务。例行修改可选择速度更快的模型，复杂工作则选择能力更强的模型。

上下文仍然和基础设施一样重要。清楚说明想要构建**什么**、**为什么**以及**如何**构建，会显著影响输出结果。

## 深入探索

你已完成核心工作流。Copilot CLI 还有一些值得了解的功能：

- `/review`：请求代码审查代理分析更改。
- `/rubber-duck`：讨论问题并获取另一种观点。
- `/fleet`：协调多个相互独立的子任务并行执行。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务发送给 Copilot cloud agent。

## 后续步骤

提升任何工具使用能力的最佳方式就是持续使用！可以用它处理生产代码、个人项目，或者那个想了很多年却一直没动手的小应用。与团队分享经验，也向团队成员学习。还可以继续探索文档。

如果想从终端扩展 Tailspin Toys，请继续学习可选的 [Foundry Backer Concierge 系列][foundry]。想比较其他工作环境，请探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
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
