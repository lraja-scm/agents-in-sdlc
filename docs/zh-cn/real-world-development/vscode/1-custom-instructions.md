---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须按照特定方式完成，或者 Copilot 应了解某些背景信息，就需要确保它能获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中最重要的是[说明文件][instruction-files]，它们可以指导代码生成。

在本练习中，将：

- 了解项目特定上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定的说明文件传递给 Copilot。
- 在当前说明生效时，生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和筛选切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全符合你设定的标准。Copilot 的结果具有概率性。本练习旨在观察更新说明前后的行为趋势，而不是要求输出逐字一致。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，其中包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式，并具有现代风格。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件的开头都应有一段注释，介绍文件的用途。

通过使用说明文件，可以确保 Copilot 获得正确的信息，并按照这些实践完成任务。

### 自定义说明

自定义说明为 Copilot 提供上下文和偏好，让它更好地理解你的编码风格和要求。这项功能可以帮助你引导 Copilot 提供更符合需求的建议和代码片段。你可以指定偏好的编码规范、库，以及喜欢的注释类型。你可以为整个存储库或特定文件类型创建说明，为任务提供上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：适用于此存储库中**每个**请求的单个说明文件。该文件应包含项目级信息，即适用于大多数 Copilot 聊天或 CLI 请求的上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型提供指导的文件。你可以为特定语言（例如 TypeScript 或 Astro）或任务（例如创建 UI 组件或一组单元测试）提供指导。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 生成代码，不适用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [还支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理说明文件的最佳做法

完整讨论如何创建说明文件超出了本工作坊的范围。不过，示例项目展示了一种有代表性的做法。总体而言：

- 将 `copilot-instructions.md` 的内容限制在项目级指导，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件针对文件类型（单元测试、Astro 组件、数据层）或特定任务提供说明。
- 使用自然语言。保持指导清晰，并提供代码应如何编写或不应如何编写的示例。

说明文件没有唯一的编写方式，AI 的使用方法也不止一种。通过实践，你会找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有完善的说明文件集合。浏览项目中的说明文件时，可能会看到针对多种任务的文件，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面中的操作各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`；对应工作流的课程会在相关位置介绍。
>
> 需要模板或起点？请探索 [awesome-copilot][awesome-copilot]，其中包含说明文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览项目中的自定义说明文件

花些时间阅读此存储库自带的说明文件：其中有一个核心 `copilot-instructions.md` 文件和一组用于不同任务的 `*.instructions.md` 文件。请在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 下还有一部分 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览文件。你会看到 Astro、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了相对于存储库根目录的 glob 模式，用于确定说明适用的文件。这里，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都符合条件。
5. 阅读此项目的单元测试创建说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将大型说明集拆分为可复用的小文件，并向 Copilot 提供可遵循的示例。（这些路径相对于说明文件，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求在代码中添加文档注释。接下来的步骤会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为本次更改创建分支。

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新的终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。每项练习可能使用不同模式和模型，相关步骤会具体说明。

为观察自定义说明的影响，先在现有说明生效时发送提示。之后更新说明，再次发送相同提示以比较差异。

1. 关闭之前练习留下的编辑器标签页，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的代理下拉列表中选择 **Agent**，以便 Copilot 应用文件更改。

   ![显示 Chat 视图中代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试文件。
6. 留意建议的辅助函数是类型化函数，将 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定（该说明适用于 `src/lib/*.ts`）。
7. 注意，建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的结果具有概率性，因此它也可能在没有明确说明的情况下添加文档注释。如果发生这种情况也没有问题；本练习重点是观察更新说明后行为是否更加一致。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为存储库添加编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常位于第 27 行附近。留意其中记录了项目编码标准，但尚未包含代码内文档规则，这就是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如前一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建说明，也可以用 `*.instructions.md` 文件针对特定语言、文件类型或任务提供指导。存储库级文件适合放置你刚添加的文档注释规则等全局项目标准。

## 再次运行提示并观察变化

说明文件已更新，现在使用相同提示再次生成辅助函数，以观察 Copilot 的建议有何变化。

1. 返回 Copilot Chat，选择 **New Chat** 清空对话缓冲区。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦到正确文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意此时生成的文件开头是否包含类似以下注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意生成的函数是否包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用到实际代码中；下一练习将在此基础上继续。

## 提交说明并推送分支

说明文件和其他项目资产一样，都应通过源代码管理进行维护。现在将说明更新和辅助函数提交并推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），打开新的终端窗口。
2. 在终端运行以下命令，确认说明更新和辅助函数更改都已存在：

   ```bash
   git status
   ```

3. 在终端暂存并提交说明更新，以及 Copilot 为辅助函数基础实现修改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更改（例如测试），也将这些文件添加到提交中。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，应创建拉取请求并关联待办事项中的文档标准议题。可以手动操作，但也可以使用已连接到项目的 GitHub 工具让 Copilot 代为处理。请提示 Copilot 查找议题、创建用于关闭该议题的 PR，然后合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已将 GitHub 工具连接到工作区，因此无需设置。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub。按照提示授权即可。稍后的练习会介绍此连接的工作方式（模型上下文协议）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux），打开 Copilot Chat 视图，并确认代理选择器中选中 **Agent**。
3. 请求 Copilot 查找与更新说明文件有关的议题，并从当前分支创建 PR，说明其中包含说明更新和新的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 如果提示 **Allow** Copilot 代表你执行 GitHub 操作，请检查命令并酌情选择 **Allow**。
6. PR 创建后，使用以下提示请求 Copilot 合并 PR，并让本地分支返回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果提示 **Allow** Copilot 代表你执行 GitHub 操作或运行 shell 命令，请检查命令并酌情选择 **Allow**。

你已在 GitHub Copilot 的帮助下创建并合并拉取请求！

## 总结和后续步骤

你已了解 Copilot 如何从项目说明文件获取上下文，并在 VS Code 中使用 Copilot Chat 完成以下任务：

- 发送代码生成提示，观察现有说明下 Copilot 的输出。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次发送相同提示，观察建议代码如何采用新标准。
- 将说明更新和辅助函数基础实现提交到 `main`，供下一练习使用。

接下来，在[代理模式][next-lesson]中使用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳做法][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 条建议][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
