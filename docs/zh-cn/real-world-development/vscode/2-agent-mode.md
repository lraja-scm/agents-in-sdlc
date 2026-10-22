---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发者的工作流程一般包括查找所需文件、进行更改、运行测试、调试、找出遗漏的文件、再次更新……这样的工作会不断重复。

这正是 Copilot 代理模式发挥作用的地方。

Copilot 代理模式旨在 IDE 中自主执行任务。它的工作方式类似开发者：先探索现有项目结构，再进行必要更新、运行测试等任务，并自动修复发现的问题。现在来看看如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和 cloud agent 名称相似，但适用于两种不同的体验。代理模式在 IDE 中执行任务，便于快速反馈和交互；cloud agent 则像团队中的同伴开发者一样异步工作，通过议题和拉取请求与你交互。

本练习将了解：

- Copilot 代理模式如何探索项目、识别相关文件并协调修改。
- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能。
- 如何在合并到代码库之前审阅 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断增加，你希望允许用户按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 代理模式完成数据层、UI 和测试方面的剩余工作。

## 运行 Tailspin Toys 网站

更改代码前，先浏览 Tailspin Toys 网站，了解其当前功能。

该网站是带有开发者主题的桌游众筹平台，用户可以浏览游戏列表和游戏详情。它是一个 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，不使用单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则使用标准 DOM API 编写的小型、作用域受限的 Astro `<script>`。

### 启动网站

项目提供了 `npm run dev` 任务，可启动单个 Astro 开发服务器。请在 GitHub Codespace 中按以下步骤运行：

1. 返回 Codespace。上一练习结束后应该已回到 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示包含 URL 的横幅，类似如下内容：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现对话框，提示打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭它。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中，从终端选择 localhost URL 时，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通往 Codespace 的专用隧道，网站服务器现在由 Codespace 托管。

### 浏览网站

网站运行后，可以探索其功能。主要功能包括：

- **主页**：显示桌游列表及其标题、图片和描述。
- **游戏详情页**：选择游戏后，会进入详情页，查看游戏标题、描述、发行商和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。每项练习可能使用不同模式和模型，相关步骤会具体说明。

网站初始版本已可正常使用，但还可以添加新功能。先查看待办事项。使用模板设置项目时，系统已自动创建了一组议题；现在请 GitHub Copilot 显示这些内容。

1. 在 Chat 视图的代理下拉列表中选择 **Agent**。**Agent** 代理可以自主规划并跨文件实现更改、运行终端命令并调用工具。

   ![显示 Chat 视图中代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并未偏好某个模型。制作工作坊时使用了 Claude Sonnet 4.5，因此将其写入说明，希望尽量保证代码建议的一致性和良好体验。不过，由于大型语言模型（LLM）具有概率性，收到的建议可能与示例不同。这完全正常。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 审阅生成的议题列表。

注意，Copilot 还根据对用户的帮助程度为议题排定了优先级。

## 审阅说明文件

让代理生成代码前，先查看将为 Copilot 工作提供上下文的说明文件。这里将使用[用户界面（UI）说明文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中包含为网站添加功能的指导。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意添加功能时的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向不同文件类型的专用说明文件链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置，因此可以复用内容、拆分大型说明，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要对应用进行三项不同更新：

- 在数据层（`src/lib/`）添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，加入筛选 UI。

此外，合并到代码库之前还必须运行测试并确认通过。Copilot 代理模式可以完成这些任务！现在添加此功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 `ui` 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉列表中仍选择 **Agent**。

   ![显示 Chat 视图中代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 确认当前模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，请 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 开始探索项目并查找相关文件。它应该会找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项任务，例如探索项目、修改文件和运行测试。根据任务复杂度和代码库规模，可能需要几分钟。过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会有 **Keep** 或 **Undo** 选项，因此无需在工作进行中选择。

7. 按 Copilot 提示选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中要求确认运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至有测试失败。这没有问题！Copilot 会在生成代码和运行测试之间反复迭代，直到任务完成且未检测到错误。

   ![Copilot 代理模式完整 Chat 会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，确认没有潜在问题。

> [!CAUTION]
> 务必审阅 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行的网站浏览器，探索新功能。
11. 确认功能正常并审阅代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能。你了解了如何：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能。
- 使用 Copilot 代理模式探索项目、识别相关文件并协调修改。
- 在合并到代码库之前审阅 Copilot 代理模式生成的更改和测试。

现在使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其打开拉取请求。

### 额外探索练习 — 实现分页

随着游戏列表不断增加，需要实现分页。使用本练习中学到的技能，请 Copilot 更新网站以实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用已有说明文件。
- 考虑分页方式，例如允许用户选择每页条目数，还是使用固定值。
- 确保提示提供 Copilot 所需的指导，以按预期创建实现。
- 可能需要与 GitHub Copilot 迭代协作，要求修改并提供上下文。这是使用 Copilot 的正常流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及使用时机][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
