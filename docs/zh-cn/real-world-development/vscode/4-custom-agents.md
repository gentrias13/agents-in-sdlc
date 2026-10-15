---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是可针对开发工作流中的特定任务或领域进行定制的 AI 助手。你可以在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，为 Copilot 提供专注的指令、最佳实践、编码模式和领域知识。团队可以将专业知识整理为可复用的智能体，例如确保符合 [WCAG][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹或全局 `~/.copilot/agents` 中的 Markdown 文件定义。每个文件都包含至少具有 `name` 和 `description` 字段的 YAML frontmatter，后面是定义智能体行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能

自定义智能体和[智能体技能][agent-skills-concept]有一些相似之处。它们主要都通过 Markdown 文件定义，并指导 AI 执行操作。可以这样区分：**自定义智能体**负责工作，**技能**则是工具。

自定义智能体拥有自己的上下文窗口，并可在工作过程中协调技能甚至其他智能体。本实验中，无障碍自定义智能体会根据无障碍指南审查并更新站点；执行任务时，它可以调用 pull request 工作流技能或运行并管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。需要针对你的环境和场景进行测试和迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前深色主题的文本与背景对比度不足，难以阅读。设计团队因此要求实现可由用户切换的高对比度模式。接下来使用自定义智能体尽快实现此功能。

本练习将介绍如何：

- 审查现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 审查无障碍自定义智能体

返回 codespace，打开终端，并从 `main` 创建一个新分支来单独开展无障碍工作（这样就能将练习 3 的筛选功能 PR 与此工作分开）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览后续各节，了解生成无障碍网站代码时的核心职责、无障碍最佳实践、HTML、CSS 和 JavaScript 示例，以及常见问题。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，聊天会话就会遵循其说明和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace 并打开 Copilot Chat 面板，然后选择 **New Chat** 开始新的对话。每项练习使用的模式和模型可能不同，相关步骤会在适用时说明。

1. 如果聊天视图中尚未选择 **Agent**，请从智能体下拉菜单中选择它。

   ![显示聊天视图智能体选择器的屏幕截图](../../../_images/shared-chat-mode-selector.png)

2. 选择聊天视图底部的智能体下拉菜单（默认显示当前智能体 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    为站点添加高对比度模式。用户应能通过切换开关启用或关闭此模式，并将设置保存在浏览器的 local storage 中，以便重新加载页面后仍然生效。
    ```

5. Copilot Chat 会探索代码库、提出更改并编辑项目文件。每项编辑都会在聊天中显示文件路径和差异，供你审查。
6. 应用编辑时，**Files changed** 指示器会更新，以显示智能体修改的文件。

> [!NOTE]
> 这可能需要几分钟。Copilot 会实际修改存储库中的文件，例如 Astro 组件、CSS 和相关测试。

下一课将审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即针对特定任务和领域定制的 AI 助手。借助自定义智能体，可以将团队的专业知识和标准整理为可复用的智能体，更有效地指导 Copilot 完成特定类型的工作。

本课介绍了以下概念：

- 自定义智能体的定义方式。
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来[监控并引导智能体的工作][next-lesson]，在更改生成过程中进行审查，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 中的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
