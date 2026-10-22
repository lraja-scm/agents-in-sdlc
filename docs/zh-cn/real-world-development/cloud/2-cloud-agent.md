---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因时间不足而长期搁置的功能请求。GitHub Copilot cloud agent 可以自主执行代码更新和功能添加等任务。代理完成工作后，会生成一个草稿 PR，供开发者审阅。这样可以将繁琐任务委派出去，加快开发进程，让开发者专注于更宏观的工作。

本练习将探索 Copilot cloud agent 的以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确限定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些亟待处理的技术债务。最初受聘创建网站的承包商留下了不理想的文档现状——实际上，完全没有文档。团队希望首先为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望在每款游戏的详情页中显示相关游戏，也就是同一类别中的其他游戏，方便用户继续浏览。目前不需要精致的实现，只需提供可用于验收测试用户体验的版本。这项工作当前受到阻碍，但还有其他优先级更高的议题。

这两类任务都很容易被降低优先级，因此非常适合交给 Copilot cloud agent。它可以在后台异步处理任务，让开发者专注于其他工作，之后再回来审阅 Copilot 的成果并确认一切符合预期。

## 介绍 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发者一样在后台执行任务。和人类开发者一样，可以通过多种方式安排工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。你可以在任务进行中或完成后查看 Copilot 会话。解决方案准备好供你审阅时，Copilot 会在拉取请求中提及你。

## 合理限定说明范围的重要性

GitHub Copilot 并没有魔法——使用它并不能跳过思考。即使看似简单的操作，深入分析后也可能相当复杂，因此应谨慎限定 Copilot cloud agent 的任务范围，并参考[最佳实践][cloud-agent-best-practices]。应将它视为 AI 配对编程助手：分阶段开展工作、学习、试验并逐步调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义说明

前面的练习介绍了自定义说明及其如何引导 Copilot。如果你已经完成了自定义说明练习，就见过 `.github/copilot-instructions.md` 的实际效果；如果还没有，现在可以快速查看。将工作分配给 Copilot cloud agent 之前，请只读浏览存储库中已有的说明文件，以便稍后观察它们的影响。

请在存储库的 GitHub 网页界面中打开以下文件，或在已运行的 Codespace 中打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是关于 TypeScript 约定和 TSDoc 文档注释的要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它会将说明限定到 `**/*.test.ts` 文件。

在下一节中，将 `Code lacks documentation` 议题分配给 cloud agent 时，请留意生成的拉取请求中是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。它们都来自这些说明文件。稍后审阅 PR 时还会再次提到这一点。

## 设置 Copilot cloud agent 的开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保一切准备就绪。将任务分配给 Copilot 时也是如此，因为它会以类似软件工程师（SWE）的方式工作。

Cloud agent 使用 [GitHub Actions][github-actions] 环境执行工作。你可以通过创建[专用设置工作流][setup-workflow]来自定义环境，并在 `.github/workflows/copilot-setup-steps.yml` 中配置，使其在 Copilot 开始工作前运行。这样可以为它提供必要的开发工具和依赖项。本实验已预先配置此工作流，以便顺利完成实验并进行学习。它确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及为单个 Astro 应用迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你在存储库的“Actions”选项卡中测试这些设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果设置步骤中不克隆存储库，步骤完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 — Node.js（整个应用现为 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

这个文件看起来和其他 GitHub 工作流类似，但有几个要点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 留意 `workflow_dispatch` 触发器。它允许你在存储库的 **Actions** 选项卡中手动运行工作流。这样可以验证工作流能否成功运行，而不必等待 Copilot 启动它。

## 添加文档

大家都知道文档的重要性，但大多数项目的信息要么已过时，要么完全缺失。这种技术债务往往得不到处理，会降低工作效率，也让维护代码库或帮助新开发者熟悉团队变得更加困难。Copilot 擅长生成文档，因此这是交给 Copilot cloud agent 的理想任务。它会在后台生成所需文档，之后的练习会回来审阅成果。

1. 在新的浏览器标签页中前往 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，创建新议题。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 议题分配详细信息](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），标题应类似 `[WIP]: Code lacks documentation`。如果列表中没有出现新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定最佳处理方案。

13. 检查列表和计划执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并确定合适方案，以及创建代码。

这可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让我们专注于其他工作。接下来将利用这一特点：一边再分配一项任务给 Copilot cloud agent，一边编写应用代码以添加功能。

## 在游戏详情页添加相关游戏区域

如前所述，GitHub Copilot cloud agent 的一大优势是可以并行分配工作，让你专注于一组任务，同时由它处理另一组任务。为设计团队添加相关游戏区域也许用不了太久，但这段时间仍可用于其他工作。现在将此任务分配给 Copilot cloud agent！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，你应该会在议题的第一条评论中看到一组 👀，表示 Copilot 已开始处理！

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理第二项请求！Copilot cloud agent 的工作方式类似软件工程师，因此不必持续监控；完成后再审阅即可。现在来创建并使用自定义代理。

## 总结和后续步骤

本练习介绍了 [GitHub Copilot cloud agent][copilot-agents]。使用 cloud agent 可以将议题分配给 Copilot，由它异步执行。你可以用 Copilot 处理技术债务、创建新功能，或协助将代码从一种框架迁移到另一种框架。

你探索了以下概念：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确限定议题范围的重要性。
- 将议题分配给 Copilot。

Copilot cloud agent 正在后台工作，现在可以开始创建并使用自定义代理。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并且能够读取自定义说明；前面已介绍过后者。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
