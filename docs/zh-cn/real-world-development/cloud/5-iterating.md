---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 合作完成了多个任务，以改进用户体验并添加功能：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式切换等无障碍功能。现在来查看代码更改；如有需要，向 Copilot 提供反馈以改进结果。

### 场景

正如前面反复强调的，引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终要审查生成的代码，并遵循常规 DevOps 流程。因此，在为团队开启审查之前，先查看 GitHub Copilot 对添加文档、相关游戏功能和无障碍功能的建议。

## 安全性与 GitHub Copilot 云智能体

Copilot 云智能体会在没有监督的情况下异步执行任务，因此需要遵守一些安全限制：

- Copilot 对存储库仅有读取权限，并且**只有**对其工作分支有写入权限。
- 云智能体在 GitHub Actions 中运行，并在单独的一次性环境中工作。
- 任何 GitHub Actions 工作流都必须经过人工批准才能运行。
- [默认情况下，云智能体对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也在限制之列。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个 pull request (PR)，该 PR 为代码添加了文档。你将使用 GitHub.com 的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。你可以选择[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 在新的浏览器标签页中打开 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 查看 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包含新建的 TSDoc 文档注释和其他文档。具体更改可能有所不同。留意 TSDoc 文档注释、TypeScript 规范和文件注释头，这些要求来自你在练习 2 开始时查看的自定义说明文件。
6. 确认更新无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应显示在 PR 的检查部分中。若一切正常，单个 Astro 应用的项目检查会通过。这可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向过程。你可以像在 pull request 或代码行内评论中 @ 提及其他团队成员一样，在评论中提及 Copilot。Copilot 会读取评论，并启动新会话来处理反馈。由于结果具有非确定性，无法提供固定提示文本。可以要求 Copilot：

- 在每个代码文件顶部添加简短说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论并提及 **@copilot**，请求它更改生成的文档。可以采用上述建议，或提出你希望 Copilot 在代码库中实现的其他文档改进。
2. 选择 **View Session** 观看 Copilot 执行任务。Copilot 会启动新会话来完成更新。
3. 选择 **Back to pull request** 返回 PR。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直到结果满足要求。满意后，可以将 PR 从草稿转换为待审查状态，然后合并到 main 分支。

![将 PR 转为待审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来查看 Copilot 为“在游戏详情页显示相关游戏” issue 创建的 PR。

1. 在 GitHub.com 上打开存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更新无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应显示在 PR 的检查部分中。若一切正常，单个 Astro 应用的项目检查会通过。这可能需要几分钟。
9. **可选：**可以切换到 Copilot 创建的分支，在 codespace 中手动测试相关游戏功能。前往 codespace，打开终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！和之前一样，可以继续迭代并请求更改，例如调整显示的相关游戏数量，或要求添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时还没有更新自定义说明）。在 **Conversation** 选项卡中添加评论，Copilot 就会收到请求并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。这个 PR 应包含练习 3 中分配的高对比度模式，以及练习 4 中在任务控制中心追加的浅色模式。

1. 在 GitHub.com 上打开存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 检查实现，重点关注：
   - 用于切换模式的 UI 组件
   - 用于保存用户偏好的本地存储
   - 高对比度和浅色模式的 CSS 或样式更改
   - 无障碍属性（ARIA 标签、键盘导航等）
   - 管理模式切换的 JavaScript/TypeScript 代码
6. 确认更新无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应显示在 PR 的检查部分中。若一切正常，单个 Astro 应用的项目检查会通过。这可能需要几分钟。
10. **可选：**可以切换到 Copilot 创建的分支，在 codespace 中手动测试无障碍功能。打开终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名称：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    随后启动应用，在浏览器中测试高对比度和浅色模式切换，确认它们正常工作，并且在重新加载页面后仍保留用户设置。

注意无障碍自定义智能体如何引导 Copilot 按照无障碍最佳实践实现功能。如果发现无障碍问题或改进点，可以像之前一样在评论中提及 **@copilot**，请求它更新。

## 可选练习——继续委派工作

云智能体适合处理真实积压工作，让你可以专注于其他任务。可以在存储库中创建更多 issue 并分配给 Copilot，以培养这种工作方式。例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成云智能体环境的练习。在这些课程中，你：

- **检查了存储库附带的自定义说明文件**，以便之后观察这些说明对云智能体输出的影响。
- **将 issue 分配给 Copilot 云智能体**，并观察它设置环境、规划并异步执行任务。
- **创建并使用自定义智能体**处理无障碍功能，添加高对比度和浅色模式切换。
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导无障碍会话。
- **审查并迭代改进云智能体的 pull request**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

云智能体环境的练习已完成。你可以继续探索其他环境，巩固所学内容：

- 🖥️ **[VS Code 环境](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率 issue 分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈配置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
