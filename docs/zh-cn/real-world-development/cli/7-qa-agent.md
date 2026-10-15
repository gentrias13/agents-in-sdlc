---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，结合 issue 要求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并使用 Playwright MCP 在浏览器中查看筛选体验。现在将这些能力整合到具有明确 QA 流程的自定义智能体中。

本课将介绍如何：

- 了解自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，并根据筛选 issue 审查其发现。

## 场景

Tailspin Toys 希望在创建 pull request (PR) 前，以一致方式审查要求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程并生成可复用报告。

自定义智能体是在 Markdown 配置文件中定义的 Copilot 专用角色。存储库说明描述团队标准，`quality-checks` 技能封装可重复检查，Playwright MCP 提供浏览器工具，QA 配置文件则指导 Copilot 如何使用这些能力评估要求并报告结果。

## 创建 QA 配置文件

1. 返回 Codespace，确认筛选功能对话处于 Interactive 模式。
2. 要求默认智能体创建 QA 配置文件：

   ```plaintext
   在 .github/agents/qa.agent.md 中创建名为 QA 的自定义智能体。它应根据对应 issue 和已确认的要求检查功能，遵循存储库说明，运行 quality-checks 技能，使用 Playwright MCP 验证行为，并在测试覆盖不足时添加测试。

   要求它将每项要求报告为通过、失败或受阻，并提供支持证据。更改实现代码前必须先询问，并且不得提交更改或创建 pull request。

   现在只创建配置文件，供我审查。
   ```

## 检查配置文件

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 查看 frontmatter。`description` 是必需项；`name` 是可选项，但可以作为清晰的显示名称。
3. 确认配置文件要求 QA 从需求开始，遵循存储库说明，运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据，修改实现代码前会先询问，并且不会提交更改或创建 pull request。
5. 如果配置文件遗漏上述职责或边界，请让默认智能体修改后再继续。

## 根据 issue 运行 QA

创建配置文件后，恢复筛选功能对话并选择 QA，让它可以使用上下文中已有的 issue 和规划决定。

1. 输入以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会出现在可用列表中，但仍可通过上述命令启用。

2. 使用以下提示审查功能：

   ```plaintext
   根据 issue 和计划中的决定审查筛选功能。它是否已准备好创建 PR？
   ```

3. 等待 QA 智能体完成工作并查看报告。

## 总结和后续步骤

你已将专用角色纳入工作流，并审查了它的工作。本课中，你：

- 了解自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查了可复用的质量保证配置文件。
- 选择了 QA 智能体，并根据筛选 issue 审查其发现。

现在实现、技能更新、QA 配置文件、测试和验证报告都已准备好，可以进入审查流程。接下来[创建功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
