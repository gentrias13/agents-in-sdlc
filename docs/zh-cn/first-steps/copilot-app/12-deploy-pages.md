---
title: "第 12 课 - 使用 GitHub Pages 部署站点"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审查 pull request 并验证 Space Quiz 是否已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

为 Space Quiz 创建一个公开访问的站点，并使用 GitHub Pages 部署。Copilot 可以准备工作流并引导部署，但你仍需负责审查更改并确认站点已上线。

本课将介绍如何：

- 让 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和 pull request。
- 跟进部署并验证已发布的站点。

## 场景

Space Quiz 已经位于 GitHub 存储库中，但其他人还没有简单的体验方式。使用 GitHub Pages 发布这个单文件站点，即可通过公开 URL 访问，无需添加服务器或运行时依赖项。

## 让 Copilot 部署站点

在 `space-quiz` 的 Copilot app 会话中发送以下提示：

```plaintext
为此存储库启用 GitHub Pages。如果需要，请创建 GitHub Actions 工作流来构建网站并部署到 GitHub Pages。如果存储库中还没有 README，请添加一个文件，介绍此应用以及如何在本地运行。使用这些更改创建 pull request，并在所需检查通过后合并。持续监控部署，直到成功，然后报告在线网站的最终 URL。如果你没有启用 Pages 或合并 PR 的权限，请明确告诉我需要批准或执行什么操作；网站上线前不要报告成功。
```

![三阶段示意图：已合并的 space-quiz pull request；成功运行的 GitHub Actions Deploy website 工作流，其中包含 Build website、Upload artifact 和 Deploy to Pages 步骤；以及位于 yourname.github.io/space-quiz/ 的 Space Quiz 在线站点。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 此提示会要求智能体创建并合并 pull request，以及更改存储库的 Pages 设置。继续前，请审查差异、部署权限和任何 GitHub 审批提示。

## 审查工作流并验证部署

1. 确认只有在存储库尚无 README 时才添加 README。
2. 审查 pull request 中的更改，包括工作流的构建和部署步骤。
3. 仅在所需的 pull request 检查通过后合并。
4. 跟进 GitHub Actions 部署运行，直到成功。
5. 打开报告的 URL，确认 Space Quiz 可以正常加载。

如果智能体无法启用 Pages 或合并 pull request，请按照它提供的具体说明操作。部署成功且在线站点可以加载后，才能认为任务已完成。

## 总结和后续步骤

Space Quiz 现在已作为在线站点发布，并且你已经亲自验证部署成功，而不是只依赖智能体的报告。继续学习[第 13 课：审查和后续步骤][next-lesson]。

[next-lesson]: ../13-review/
