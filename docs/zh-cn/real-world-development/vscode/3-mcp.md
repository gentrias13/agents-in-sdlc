---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。在创建 pull request 前，应先确认它在浏览器中正常运行。现在连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。

本练习将介绍如何：

- 了解模型上下文协议 (MCP) 及 MCP 服务器如何扩展 Copilot 工具。
- 在工作区添加 Playwright MCP 服务器。
- 让 Copilot 使用服务器在浏览器中手动测试筛选功能。
- 发布分支，并为筛选工作创建 pull request。

## 什么是模型上下文协议 (MCP)？

智能体模式连接编辑器之外的工具后会更加强大。模型上下文协议 (MCP) 是 Copilot 与外部工具和服务交互的标准方式。

![显示智能体模式内部工作方式及其与上下文、LLM 和工具交互方式的示意图，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时访问外部工具和服务，使用资源获取最新信息，并通过工具代你执行操作。

MCP 服务器充当 AI 智能体与外部工具及服务之间的桥梁，负责管理双方的通信，例如访问现有 API 或 NPM 包提供的本地工具。每个 MCP 服务器都提供一组 AI 智能体可以访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP 服务器](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，可创建和更新存储库、管理 issue 和 pull request。
- **[Playwright MCP 服务器](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能，可导航网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可用于访问不同工具和资源。GitHub 托管了 [MCP 注册表](https://github.com/mcp)，帮助用户发现生态系统中的服务器并参与贡献。

> [!CAUTION]
> 对待 MCP 服务器时，应像对待项目中的其他依赖项一样。使用前请仔细审查源代码、验证发布者并考虑安全影响。只使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应该看到已配置的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

此 `github` 配置来自项目模板，因此 Copilot 能在上一练习中读取 issue 积压工作。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装；VS Code 会通过 HTTP 连接，并在 Copilot 首次调用工具时使用 GitHub 完成身份验证。

## 添加 Playwright MCP 服务器

接下来添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控制的浏览器，用于测试刚构建的功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 项，与 `github` 并列，文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志可让 Playwright 在没有可见窗口的情况下运行浏览器；codespace 中没有可显示窗口的桌面环境，因此需要使用此标志。

> [!NOTE]
> Tailspin Toys 项目已经使用 Playwright 进行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会在需要时启动 MCP 服务器。Copilot 首次需要某个服务器提供的工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 配置上方会出现 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任此服务器，请先审查配置，再选择信任以启动服务器。

手动启动不是必需步骤。如果跳过，VS Code 会在下一步中 Copilot 首次需要这些工具时启动服务器，并提示你确认是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]让 Copilot 可以驱动真实浏览器。无需亲自选择页面上的控件来验证功能，智能体可以打开页面、导航、应用筛选并读取结果，然后向你总结观察到的内容。这样无需离开对话，就能快速确认功能是否符合预期。

Playwright MCP 服务器会读取页面的[无障碍树][playwright-mcp-server]，而不是屏幕截图。因此，智能体会根据带标签的结构化元素（例如按钮、链接和列表项）进行推理，就像辅助技术一样。快速功能检查也能顺便验证一些基础无障碍行为。

连接服务器并启动应用后，要求 Copilot 测试筛选功能：

```text
使用 Playwright MCP 服务器，在浏览器中打开运行于 http://localhost:4321 的应用，并验证新的游戏筛选功能：

1. 前往游戏页面并记录列表中的游戏数量。
2. 应用类别筛选，确认列表仅显示该类别中的游戏。
3. 清除筛选，然后应用出版商筛选，确认列表更新为该出版商的游戏。
4. 同时应用类别和出版商筛选，确认结果同时符合两项条件。

报告每个步骤的观察结果，并指出任何不符合预期的行为。
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐步执行测试并汇报结果。将报告与 issue 中的验收标准进行对照。如果发现异常，请继续询问或让 Copilot 在创建 pull request 前修复代码。

> [!NOTE]
> 测试时应用必须在 `http://localhost:4321` 运行。如果开发服务器已停止，请先重新启动。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建 pull request

确认功能正常后，可以创建 pull request (PR) 供团队审查。首先发布 `filtering-vscode` 分支：

1. 在 codespace 中打开 **Source Control** 面板并审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板屏幕截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish** 将分支推送到存储库。

## 创建 pull request

可以通过 github.com 或 GitHub 命令行界面 (CLI) 创建 pull request。这里让 Copilot 帮助创建 PR，并关联相应 issue：

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确保已在智能体下拉菜单中选择 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 要求 Copilot 创建 PR：

   ```plaintext
   查找存储库中与按类别和出版商筛选有关的 issue。为当前分支创建新的 pull request，并将其关联到正确的 issue。
   ```

4. 根据提示选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，可能会提示你通过 GitHub 身份验证；按照提示完成操作。
5. 观察 Copilot 如何搜索 issue、找到正确条目并创建 PR。
6. 选择 Copilot 提供的链接审查 pull request，但**不要**合并。

## 总结和后续步骤

恭喜你！本练习中，你：

- 了解了模型上下文协议 (MCP) 及 MCP 服务器如何扩展 Copilot 工具。
- 在工作区添加了 Playwright MCP 服务器。
- 使用该服务器在创建 pull request 前通过浏览器手动测试筛选功能。
- 发布了分支，并为筛选工作打开了 PR。

你使用 MCP 服务器测试了功能，但 MCP 也是扩展 Copilot 的方式。接下来[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [GitHub MCP 服务器][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
