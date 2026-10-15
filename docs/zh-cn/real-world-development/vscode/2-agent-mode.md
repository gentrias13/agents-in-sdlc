---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并运行测试。开发人员需要查找相关文件、修改代码、运行测试、调试、发现遗漏后再次修改。Copilot 智能体模式可以简化此过程。

智能体模式可以在 IDE 中更自主地工作。它会像开发人员一样先探索项目结构、执行必要的更改、运行测试等任务，并自动修复发现的问题。接下来使用智能体模式为站点添加新功能。

> [!NOTE]
> 智能体模式和云智能体虽然名称相似，但面向不同的场景。智能体模式在 IDE 中执行任务，便于快速反馈和交互；云智能体则像团队成员一样异步工作，通过 issue 和 pull request 与你协作。

本练习将介绍如何：

- 使用 Copilot 智能体模式探索项目、识别相关文件并协调修改。
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现功能。
- 在合并到代码库前审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表增长，需要支持按类别和出版商筛选。上一练习已添加 publishers helper，现在将使用 Copilot 智能体模式完成其余数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改前先了解 Tailspin Toys 网站当前的功能。该网站是一个桌游众筹平台，允许用户浏览游戏列表和详情。它是单个 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 frontmatter 和 Drizzle ORM 直接查询本地 SQLite 数据库；没有单独的后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互通过范围明确的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器：

1. 返回上一练习使用的 codespace，此时应位于 `main` 分支。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）在 Codespace 中打开终端。
3. 创建并切换到用于筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示包含 URL 的横幅，内容类似：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现询问是否打开 `http://localhost:4321` 的对话框，请选择 **x** 关闭它。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），并选择 Astro 服务器地址 `http://localhost:4321`。

> [!NOTE]
> 在 codespace 中选择 localhost URL 会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是由 codespace 托管网站的私有隧道。

### 浏览网站

网站启动后，可以探索主要功能：

- **主页**：显示桌游列表、标题、图像和说明。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、说明、出版商和类别等信息。

## 使用 Copilot 查看积压工作

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace 并打开 Copilot Chat 面板，然后选择 **New Chat** 开始新的对话。模式和模型选择会因练习而异，相关步骤会在适用时说明。

网站初始版本已经可以正常使用，但还可以添加新功能。项目模板会自动创建 issue 积压工作，请让 GitHub Copilot 列出这些 issue：

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。智能体会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示聊天视图智能体选择器的屏幕截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者不偏好特定模型。编写本研讨会时使用了 Claude Sonnet 4.5，因此示例中指定了该模型，希望代码建议相对一致。但大型语言模型具有非确定性，收到的建议可能与示例不同，这是正常现象。

> [!NOTE]
> Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，查看积压工作：

   ```plaintext
   请显示我的 GitHub 存储库中的积压事项，并根据它们对用户的价值帮助我确定优先级。
   ```

4. 选择 **Continue** 以运行列出所有 issue 的命令。
5. 查看生成的 issue 列表和优先级。

## 查看说明文件

让智能体生成代码前，先查看为 Copilot 提供上下文的说明文件。项目的[用户界面 (UI) 说明][ui-instructions]提供了添加网站功能的指导。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看其中的架构概览、组件设计、可测试性和无障碍原则，以及指向 Astro、Tailwind CSS 和 Drizzle 等说明文件的链接。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。使用相对路径可复用指导内容、拆分大型说明，并链接到示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

完成筛选至少需要三项更改：在 `src/lib/` 中添加或完善筛选逻辑、添加或更新筛选测试，以及更新游戏列表页并添加筛选 UI。合并前还要确保测试通过。Copilot 智能体模式可以完成这些工作：

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 说明文件。

   ![选择 UI 说明文件的屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图中仍选择 **Agent**。
4. 确认模型仍为 **Claude Sonnet 4.5**。
5. 发送以下提示，根据积压工作中的相关 issue 实现功能：

   ```plaintext
   请根据积压工作中相关 GitHub issue 的要求，更新站点以支持按出版商和类别筛选。上一练习已经创建了 publishers helper；请保留并在需要时完善现有工作，同时完成其余数据层、UI 和测试工作。完成前确保所有测试通过。服务器已经在运行，无需重新启动。
   ```

6. 观察 Copilot 如何探索项目、查找数据层 helper、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试，这可能需要几分钟。过程中，编辑器中可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，会为所有更改提供相应操作，因此不必在工作进行时立即选择。

7. Copilot 提示运行测试时，选择 **Continue**。
8. 过程中可能会暂停，甚至出现测试失败。这是正常的。Copilot 会在生成代码和运行测试之间反复调整，直到不再检测到错误。
9. 检查生成的代码是否存在问题。

> [!CAUTION]
> 始终要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行网站的浏览器，探索新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜你！本练习使用 GitHub Copilot 智能体模式为 Tailspin Toys 添加了新功能。你了解了 Copilot 智能体模式如何：

- 跨 UI 和数据层实现新功能。
- 探索项目、识别相关文件并协调修改。
- 在合并到代码库前审查更改和测试。

接下来[使用 Playwright MCP 服务器测试功能][next-lesson]，然后为更改创建 pull request。

### 附加探索练习——实现分页

随着游戏列表增长，需要实现分页。使用本练习学到的方法，提示 Copilot 更新站点并实现分页。请遵循现有最佳实践（包括使用现有说明文件），考虑页面大小是否可由用户选择，并在提示中提供 Copilot 实现目标所需的上下文。你可能需要多次迭代、请求更改并补充上下文，这正是与 Copilot 协作的常见方式。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能与使用时机][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
