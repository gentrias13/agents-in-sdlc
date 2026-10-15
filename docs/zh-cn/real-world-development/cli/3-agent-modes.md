---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确认实现方案，使用 Autopilot 根据 issue 构建筛选功能，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

上一课为项目添加了简单功能，但较大的更改需要更完善的流程。GitHub Copilot CLI 可配合组织现有工作流，帮助以正确方式构建所需功能。本课将从 issue 开始开发功能，确认代码有效、功能符合预期，最终将更改合并到项目中。

> [!NOTE]
> 后续课程会继续使用同一个对话和分支。实际开发中通常会为不同文件类型创建不同分支或 PR；这里采用简化方式以便聚焦核心概念。

本课将介绍如何：

- 从 GitHub issue 开始新的 Copilot 对话。
- 在 Plan 模式中定义要求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续开发此功能时，还会更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断增长，访客需要按类别和出版商筛选游戏。积压工作中的 issue 描述了功能需求，但编码前需要确认组合筛选等细节。接下来使用 Plan 模式确定方案，再让 Autopilot 在限定范围内实现功能。

## 背景

引入 AI 编码智能体不会改变软件开发的基本原则。常见的开发流程包括：

1. 从描述待办事项的 issue 开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建 pull request (PR)。
7. 代码审查和持续集成成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述步骤类似。

遵循标准流程可确保 AI 生成的代码符合要求，并像手写代码一样经过验证。

## 对话模式

**对话模式**决定智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在不同模式间切换：

- **Interactive**：你与智能体协作。智能体提出更改并等待你的输入。
- **Plan**：智能体先制定计划，在此模式下无法编辑项目文件。
- **Autopilot**：智能体自主编写代码、运行测试并迭代，直到认为任务完成。

先使用 Plan 模式审查计划，再切换到 Autopilot 实现。

## 从 issue 开始

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
4. 更新 `main` 并创建筛选功能分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用方向键选择标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示并返回 **Session** 选项卡。

提示现在会以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的 issue 或 pull request (PR) 添加到上下文中。

## 规划筛选功能

在实现功能或执行任务之前，先规划实现方案可以明确预期。对于复杂工作，花时间规划总是值得的。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的 issue 引用后输入：

   ```plaintext
   为实现此功能制定计划。
   ```

Copilot 会探索项目并确定合适的实现方式，也可能会询问筛选功能应如何工作。根据偏好回答这些问题。

3. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
4. 浏览并审查计划；如有内容与决定不符，请要求 Copilot 修改。

## 批准 Autopilot

审查计划后即可开始实现。Autopilot 会持续迭代，直到 Copilot 认为任务完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中名称相近的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 根据计划生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，不要在检查未完成时认为验证已通过。

## 审查并验证实现

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff` 并审查筛选实现和测试。
3. 将结果与 issue 和规划时做出的决定进行比较。
4. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 修复失败项。

## 体验新功能

1. 要求 Copilot 启动应用：

   ```plaintext
   启动应用，以便我在浏览器中试用筛选功能。告诉我 URL，并让服务器继续运行。
   ```

2. Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按出版商筛选，以及计划中确定的组合筛选。
4. 确认重置和无结果状态符合 issue 和规划中的决定。
5. 返回 Codespace 并要求 Copilot 停止其启动的开发服务器。

## 总结和后续步骤

你已使用不同对话模式构建并审查功能。本课中，你：

- 从 GitHub issue 开始新的 Copilot 对话。
- 在 Plan 模式中定义要求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

接下来[使用自定义说明][next-lesson]，了解如何确保生成的代码遵循文档化实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
