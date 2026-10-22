---
title: "第 12 课 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审查拉取请求，并确认 Space Quiz 已成功发布。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 部署 Space Quiz，让所有人都能访问。Copilot 可以准备工作流并指导部署，但你需要负责审查更改并确认网站已上线。

本课将：

- 请求 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和拉取请求。
- 跟踪部署并验证已发布的网站。

## 场景

Space Quiz 已位于 GitHub 存储库中，但用户仍需要一种简单的体验方式。使用 GitHub Pages 发布单文件网站后，用户可以通过公共 URL 访问，无需添加服务器或运行时依赖项。

## 请求 Copilot 部署网站

在 `space-quiz` 的 Copilot app 会话中，发送以下提示：

```plaintext
为此存储库启用 GitHub Pages。创建一个 GitHub Actions 工作流，在需要时构建网站并将其部署到 GitHub Pages。如果存储库中还没有 README，请添加一个，介绍此应用并说明如何在本地运行。针对这些更改创建拉取请求，并在必需的检查通过后合并。持续监控部署，直到成功，然后报告已发布网站的最终 URL。如果你没有权限启用 Pages 或合并，请准确告知我需要批准或执行哪些操作；网站上线前不要报告成功。
```

![三阶段示意图：已合并的 space-quiz 拉取请求；GitHub Actions 的 Deploy website 运行成功，包含 Build website、Upload artifact 和 Deploy to Pages 步骤；以及发布在 yourname.github.io/space-quiz/ 的 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 此提示要求智能体创建并合并拉取请求，以及更改存储库的 Pages 设置。继续之前，请审查 diff、部署权限和 GitHub 的任何审批提示。

## 审查工作流并验证部署

1. 确认仅当存储库中没有 README 时才添加了 README。
2. 审查拉取请求中的更改，包括工作流的构建和部署步骤。
3. 仅在必需的拉取请求检查通过后合并。
4. 跟踪 GitHub Actions 部署运行，直到成功。
5. 打开报告的 URL，确认 Space Quiz 能够加载。

如果智能体无法启用 Pages 或合并拉取请求，请遵循它提供的具体说明。部署成功且网站能够加载后，才能将任务视为完成。

## 总结与后续步骤

Space Quiz 现已作为实时网站公开，你也亲自验证了部署，而不是只依赖智能体的报告。继续学习[第 13 课：回顾与后续步骤][next-lesson]。

[next-lesson]: ../13-review/
