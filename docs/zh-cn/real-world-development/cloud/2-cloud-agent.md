---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

许多组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因时间不足而长期留在积压工作中的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。智能体完成工作后会创建草稿 PR，供开发人员审查。这有助于卸下繁琐任务、加快开发进程，让开发人员专注于更重要的工作。

本课将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保操作安全。
- 明确限定 issue 范围。
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初负责网站的承包商没有为代码编写文档。团队希望先为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们想在每个游戏详情页显示同一类别中的其他游戏，让用户可以继续浏览。暂时不需要完善的实现，只要能用于验收测试用户体验即可。虽然这项工作目前受阻，但还有优先级更高的 issue。

这些任务很容易被推迟，适合交给 Copilot 云智能体异步处理。开发人员可以继续其他工作，稍后再回来审查 Copilot 的结果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发人员一样在后台执行任务。你可以通过多种方式分配工作，例如[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿 pull request 跟踪进度、设置环境并开始处理任务。你可以在会话进行时或结束后查看它的操作。解决方案准备好审查时，Copilot 会在 pull request 中 @ 提及你。

## 明确界定任务范围的重要性

GitHub Copilot 没有魔法，也不能代替思考。即使看似简单的操作，深入分析后也会发现复杂性，因此要注意[为 Copilot 云智能体清晰界定任务范围][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段开展工作，不断学习、试验和调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

之前的练习介绍了自定义说明及其对 Copilot 的引导作用。如果已完成自定义说明练习，就已经看过 `.github/copilot-instructions.md` 的效果；如果尚未查看，现在可以快速了解。将任务分配给云智能体前，先以只读方式浏览此存储库附带的说明文件，之后就能观察它们的影响。

在存储库的 GitHub Web 界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，特别关注 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，它将说明限定为 `**/*.test.ts` 文件。

下一节将“代码缺少文档” issue 分配给云智能体时，留意生成的 pull request 是否包含 TSDoc 文档注释、TypeScript 约定和注释头。这些要求来自刚才查看的说明文件。稍后的练习还会再次审查这些内容。

## 为 GitHub Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要准备特定的环境并运行设置脚本，以确保环境就绪。为 Copilot 分配任务时也一样，云智能体的工作方式与软件工程师类似。

云智能体使用 [GitHub Actions][github-actions] 创建工作环境。你可以通过 `.github/workflows/copilot-setup-steps.yml` 中配置的[专用设置工作流][setup-workflow]进行自定义，在 Copilot 开始工作前安装所需的开发工具和依赖项。本实验已预先配置该工作流，以便顺利开展学习。它会为 Copilot 安装 Node.js、项目依赖项、端到端测试所需的浏览器，以及用于单个 Astro 应用的迁移和填充完成的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为步骤所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 contents: read 权限。如果设置步骤不克隆存储库，Copilot 会在步骤完成后自动执行此操作。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（应用现为 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试可以使用数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件看起来与其他 GitHub 工作流类似，但有几个要点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 pull request 前，GitHub Actions 会运行此作业。
- 注意 `workflow_dispatch` 触发器。你可以从存储库的 **Actions** 选项卡手动运行工作流，测试它是否成功，而不必等 Copilot 启动它。

## 添加文档

每个人都知道文档的重要性，但许多项目的信息已经过时，或完全缺少文档。这类技术债务经常得不到处理，降低效率，增加维护代码库或引入新开发人员的难度。Copilot 擅长创建文档，因此很适合将此 issue 分配给云智能体。它会在后台生成所需文档；后续练习会审查结果。

1. 在新的浏览器标签页中打开 GitHub.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 创建新 issue。
5. 将 **Title** 设为 `代码缺少文档`。
6. 将 **Description** 设为：

    ```plaintext
    我们组织要求在有助于理解时，为函数和方法添加 TSDoc 文档注释。遗憾的是，最近的更新没有遵循这一标准。我们需要更新现有代码，确保在有助于说明行为时添加文档注释。
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将 Copilot 分配到 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新创建的 pull request (PR)，标题类似 `[WIP]: 代码缺少文档`。如果列表中尚未显示 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会在 PR 中创建待办事项列表。

> [!NOTE]
> Copilot 创建待办事项列表可能需要几分钟。在此期间，它正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看待办事项列表和即将执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 开始处理 issue 的通知。
15. 选择 **View session**。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态。

16. 查看实时会话，观察 Copilot 如何探索代码、了解当前状态、暂停思考并确定计划，然后编写代码。

这可能需要几分钟。Copilot 云智能体的主要优势之一就是能够异步处理任务，让你专注于其他工作。接下来将利用这一点：先让云智能体处理另一个任务，然后自己编写代码，为应用添加功能。

## 在游戏详情页添加相关游戏

如前所述，GitHub Copilot 云智能体的一项主要优势是可以拆分工作，让你和智能体各自处理不同任务。为设计团队添加相关游戏区域可能不会花很长时间，但这段时间也可以用于其他工作。现在将该任务分配给云智能体。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设为 `在游戏详情页显示相关游戏`。
6. 将 **Description** 设为：

    ```markdown
    我们希望通过在每款游戏的详情页显示相关游戏，帮助用户发现更多游戏。设计团队希望探索用户体验并进行一些验收测试。具体要求如下：

   - 在 `src/lib/` 中添加数据访问 helper，返回与指定游戏同一类别的其他游戏，但不包括该游戏本身。
   - 在游戏详情页中添加“相关游戏”部分，并使用该 helper。
   - 处理游戏没有相关游戏的情况。
   - 为新 helper 编写单元测试。
   - 创建 PR 前确保所有测试通过。
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将 Copilot 分配到 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

很快，issue 中的第一条评论下方会出现一组 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二个请求。云智能体的工作方式类似软件工程师，因此不必一直监控，等它完成后再审查即可。现在可以转而创建并使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。你可以将 issue 分配给云智能体，让它异步处理技术债务、开发新功能，或帮助将代码从一种框架迁移到另一种框架。

本课介绍了以下概念：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定 issue 范围。
- 将 issue 分配给 Copilot。

云智能体正在后台工作，接下来创建并使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并支持之前练习中介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
