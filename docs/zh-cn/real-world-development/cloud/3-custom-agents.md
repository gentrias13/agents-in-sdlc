---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是可针对开发工作流中的特定任务或领域进行定制的 AI 助手。你可以在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，为 Copilot 提供专注的指令、最佳实践、编码模式和领域知识，让它更有效地完成特定类型的工作。团队可以将专业知识整理为可复用的智能体，例如确保符合 [WCAG][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

项目级自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义；全局智能体则可放在 `~/.copilot/agents` 中。每个文件都包含至少具有 `name` 和 `description` 字段的 YAML frontmatter，后面是定义智能体行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能

自定义智能体和[智能体技能][agent-skills-concept]有一些相似之处。它们主要都通过 Markdown 文件定义，并指导 AI 执行操作。可以这样区分：**自定义智能体**负责工作，**技能**则是工具。

自定义智能体拥有自己的上下文窗口，并可在工作过程中协调技能甚至其他智能体。本实验中，无障碍自定义智能体会根据无障碍指南审查并更新站点；执行任务时，它可以调用 pull request 工作流技能或运行并管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。与 AI 的其他用法一样，需要针对你的环境和场景进行测试和迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本练习将介绍：

- 如何定义自定义智能体。
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前深色主题的文本与背景对比度不足，难以阅读。设计团队因此要求实现可由用户切换的高对比度模式。

无障碍功能至关重要，因此希望尽快实现。接下来将使用自定义智能体生成此功能。

## 审查无障碍自定义智能体

项目已经为无障碍工作创建了自定义智能体。返回 codespace 并查看其内容，了解它如何引导 Copilot：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览后续各节，了解其中有关无障碍网站代码生成的核心职责、最佳实践、HTML、CSS 和 JavaScript 示例，以及常见问题清单。

## 创建 issue 并分配任务

任务控制中心是管理环境中所有智能体的中心位置。你可以在此将任务分配给 Copilot 云智能体、监控任务，也可以重定向会话并提供额外指导。现在创建任务，让 Copilot 实现高对比度模式。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设为 `为网站添加高对比度模式`。
6. 将 **Description** 设为：

    ```plaintext
    我们需要为站点添加高对比度模式。用户应能通过切换开关启用或关闭此模式，并将设置保存在浏览器的 local storage 中。
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。
9. 从列表中选择 **Accessibility agent**。

    ![云智能体分配界面的屏幕截图，其中突出显示了自定义智能体和无障碍智能体](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即针对特定任务和领域定制的 AI 助手。借助自定义智能体，可以将团队的专业知识和标准整理为可复用的智能体，更有效地指导 Copilot 完成特定类型的工作。

本课介绍了以下概念：

- 自定义智能体的定义方式。
- 如何将任务分配给自定义智能体。

Copilot 正在实现高对比度模式。接下来前往任务控制中心，[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做好准备][org-custom-agents]
- [为企业使用自定义智能体做好准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
