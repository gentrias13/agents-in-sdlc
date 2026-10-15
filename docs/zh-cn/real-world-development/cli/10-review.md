---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个 pull request、可复用定制项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI，并完成了以下任务：

- 准备 Codespace、安装 Copilot CLI、探索项目，并找到预先创建的筛选 issue。
- 为游戏卡片添加星级评分，在转发的浏览器中审查，并手动合并第一个 pull request (PR)。
- 从筛选 issue 出发，在 Plan 模式中制定方案，在 Autopilot 模式中实现，并在 Interactive 模式中审查。
- 通过自定义说明引导智能体，自定义 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加 Playwright 模型上下文协议 (MCP) 服务器，在真实浏览器中探索筛选功能。
- 创建并选择 QA 自定义智能体，评估要求、覆盖范围、技能结果和浏览器证据。
- 审查完整筛选更改，并授权 Agent Merge 处理筛选 PR。
- 探索用于上下文、模型、共享和可选云委派的斜杠命令。

## 发布的更改

本研讨会包含两个 PR 阶段，每个阶段都从更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片中显示已有 `starRating` 和明确的无评分状态。
2. **筛选和质量工作流：**实现筛选、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并包含相关测试。

从规划筛选功能到创建 PR，你一直使用同一个对话和分支。为简化研讨会，这些工作被合并在一个 PR 中。

## 不同的验证方式

你通过自动化测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 探索浏览器等方式检查代码。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 智能体会结合这些结果审查要求和测试覆盖率，然后再创建 PR。

新增测试应弥补真实缺口；如果 QA 运行后确认无需新增测试，也可能是正确结果。授权合并前应审查代码和证据；代码更改后，要更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响其工作结果。本研讨会中，你更新了说明、自定义技能、创建 QA 配置文件并配置 MCP 服务器。可以在多个对话间复用这些定制项，并根据团队需求进行调整。说明用于设置标准，技能用于描述可重复任务，自定义智能体定义专用角色，MCP 服务器用于连接外部工具。请审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择**模式和模型**：使用 **Plan** 在开始构建前确定方案；使用 **Interactive** 处理需要持续反馈的更改；使用 **Autopilot** 处理范围明确的任务。日常编辑可选择更快的模型，复杂工作则可选择能力更强的模型。

上下文与基础设施同样重要。清楚说明希望构建什么、为什么构建以及如何构建，会显著影响输出。

## 继续探索

还可以了解 Copilot CLI 的其他功能，例如 `/review` 审查更改、`/rubber-duck` 讨论问题、`/fleet` 并行协调子任务、`/worktree` 隔离任务，以及 `/delegate` 将任务交给 Copilot 云智能体。

## 后续步骤

熟悉工具的最佳方式是持续使用它。可以将 Copilot 用于生产代码、业余项目，或一直想构建但尚未动手的小应用。与团队分享经验，也从他人那里学习，并继续探索文档。

若要从终端继续扩展 Tailspin Toys，可以学习可选的 [Foundry Backer Concierge 系列课程][foundry]。也可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理 pull request][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
