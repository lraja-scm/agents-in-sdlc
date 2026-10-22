---
title: "练习 1 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须按照特定方式完成，或者 Copilot 应了解某些背景信息，就需要确保它能获取这些上下文。[说明文件][instruction-files]可以提供此类指导，让 Copilot 不仅了解你希望它做什么，也了解你希望它如何完成。

在本练习中，将：

- 了解项目特定上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定的说明文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作流不同，这里不会运行“前后对比”提示。Copilot cloud agent 会在 GitHub 议题上异步工作，因此难以实时并排展示效果。稍后在此工作流中审阅 cloud agent 生成的拉取请求时，可以看到说明文件产生的影响。

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
2. 浏览文件，留意项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 下还有一部分 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览文件。你会看到 Astro、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` frontmatter，它设置了相对于存储库根目录的 glob 模式，用于确定说明适用的文件。这里，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都符合条件。
5. 阅读此项目的单元测试创建说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将大型说明集拆分为可复用的小文件，并向 Copilot 提供可遵循的示例。（这些路径相对于说明文件，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求在代码中添加文档注释。接下来的步骤会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来需要进行唯一一处编辑：添加项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。之后，在此工作流中将议题分配给 Copilot 时，cloud agent 就会采用这项规则。

编辑前，先创建一个分支（cloud agent 会从议题目标分支读取说明，但仍应提交自己的更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并说明更新

Cloud agent 会从议题目标分支读取说明。下一练习中将议题分配给 Copilot 时，它会从 `main` 创建分支，因此必须先将说明更改合并到 `main`，cloud agent 才能使用。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。这样，cloud agent 就能在下一练习中使用这些说明。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。这里使用分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何从项目说明文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。之后在此工作流中审阅 cloud agent 创建的拉取请求时，可以看到这项标准的实际应用。

接下来，[将第一个议题分配给 Copilot cloud agent][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳做法][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 条建议][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
