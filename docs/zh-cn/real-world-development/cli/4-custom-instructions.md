---
title: "练习 4 - 使用自定义说明引导 Copilot"
description: "了解存储库说明，添加文档标准，并将其应用到筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务必须按照特定方式完成，就需要让 Copilot 获取相应指导。[说明文件][instruction-files]不仅描述你想要什么代码，还描述代码应如何组织。现在你已构建筛选功能，接下来将了解 Copilot 使用的说明、添加文档标准，并将其应用到代码中。

在本练习中，将：

- 了解存储库说明和按路径限定的说明文件如何传递给代理。
- 更新说明文件，确保遵循编码标准。
- 观察说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，其中包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值；如果函数中有可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持更新。
- 应保留现有的格式和 lint 指南。

通过使用说明文件，可以确保 Copilot 获得正确的信息，并按照这些实践完成任务。

## 说明文件

自定义说明为 Copilot 提供上下文和偏好，让它更好地理解你的编码风格和要求。这项功能可以帮助你引导 Copilot 提供更符合需求的建议和代码片段。你可以为整个存储库或特定文件类型创建说明，为任务提供上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：适用于此存储库中**每个**请求的单个说明文件。该文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指导的文件。

> [!NOTE]
> 其他格式的说明及其支持情况因环境而异。使用特定格式前，请查看[自定义说明支持参考][custom-instructions-support]。

## 浏览项目中的自定义说明文件

项目起始模板已包含一组说明文件。先了解现有内容，再进行更改，以便观察其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意其中对项目的简要说明和编码指导。这些说明适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览文件。你会看到 Astro、Drizzle 数据层、测试等方面的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了一个 glob 模式，用于确定说明适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，留意其中指向其他说明文件和项目现有文件的链接。这样可以将较大的说明集拆分为可复用的小文件，并向 Copilot 提供可遵循的示例。

## 更新说明文件以符合团队要求

现有文件是不错的起点，但还缺少一项要求。请修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，通常位于第 35 行附近。
2. 在该部分最后添加以下项目符号：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库说明。保存说明后，退出并恢复筛选功能对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选功能对话仍处于打开状态，请输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录运行以下命令，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选功能对话，并确认已检出 `game-filters-cli` 分支。
3. 请求 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并检查已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

你已了解 Copilot 如何从说明文件获取上下文，并将新的标准应用到功能中。具体来说，你：

- 了解了存储库说明和按路径限定的说明文件如何传递给代理。
- 更新了说明文件，确保遵循编码标准。
- 观察了说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试一致地运行。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建有效自定义说明的最佳做法][instructions-best-practices]
- [Awesome Copilot — 说明文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
