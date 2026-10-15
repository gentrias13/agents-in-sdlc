---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 GitHub Copilot 云智能体执行了三个任务，改进用户体验并添加功能。云智能体虽然可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，例如 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有拥有未关闭 pull request (PR) 的智能体任务、了解执行过程，甚至为正在进行的会话提供指导。

本课将介绍如何：

- 使用智能体页面监控云智能体任务。
- 引导正在运行的会话添加功能。

## 场景

将任务分配给智能体实现高对比度模式后，团队意识到也可以借此增加浅色模式。既然正在更新站点样式并添加切换功能，加入浅色模式也很合适。现在要引导智能体，让它同时实现浅色和高对比度模式。

## 查看 GitHub Copilot 云智能体任务

查看分配给云智能体的所有任务的当前状态：

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。你应该能看到分配给 Copilot 的任务，包括更新代码文档、生成用于修改游戏的 API，以及为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如检出代码、创建工作环境、设置 MCP 服务器，以及完成已分配任务的各项步骤。

> [!NOTE]
> 显示的具体步骤会因 Copilot 工作进度和采用的方法而异。

4. 还要查看右侧的 pull request (PR) 面板。你可以在这里查看 PR 和已更改的文件，以便继续监控。

## 引导云智能体

现在已了解当前运行的任务，接下来请在智能体处理高对比度模式时，让它也加入浅色模式切换。

1. 选择与添加高对比度模式有关的会话。具体标题会因 Copilot 使用的名称和任务进度而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它完成准备工作并开始执行任务。此时会看到类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    在实现高对比度模式的同时，也添加浅色模式。该模式也应有一个切换开关，供用户选择所需的显示模式。
    ```

    ![智能体页面中突出显示“Steer active session while Copilot is working”对话框的任务屏幕截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示，并将新要求纳入工作流程。

## 让 Copilot 继续工作

Copilot 会像之前一样继续处理更新后的任务。收到消息时，它正在执行的步骤完成后，就会将新要求纳入工作流程。

这同样需要几分钟。你可以借此暂停一下，回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心控制台。通过此页面，你可以跟踪所有活动和已完成的任务、审查执行情况，并重定向正在进行的任务以调整范围或提供更多指导。

本课介绍了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在运行的会话以添加功能。

Copilot 正在完成功能开发。接下来[迭代改进 Copilot 创建的 pull request][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
