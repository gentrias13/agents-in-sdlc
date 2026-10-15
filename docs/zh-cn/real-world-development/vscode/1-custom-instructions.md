---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能够访问这些内容。[说明文件][instruction-files]可提供相关指导，帮助 Copilot 理解你希望它做什么，以及应该如何完成。

本练习将介绍如何：

- 通过存储库自定义说明和按路径限定范围的说明文件，为 Copilot 提供项目背景、编码指南和文档标准。
- 在当前说明下生成第一个筛选数据切片（publishers helper）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 重新运行相同提示，观察生成的代码如何遵循新标准。
- 将说明更新和筛选基础提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能与部分标准不完全一致。Copilot 具有非确定性。本练习旨在展示更新说明后行为的整体变化，而不是让输出与示例逐字一致。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式，并具有现代感。
- 代码中应使用 TSDoc 文档注释。
- 每个文件开头都应有一段注释，说明文件用途。

使用说明文件可确保 Copilot 获取正确的信息，并按照这些实践完成任务。

### 自定义说明

自定义说明可向 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。你可以指定编码约定、库和注释要求，并创建覆盖整个存储库或仅适用于特定文件类型的说明。

说明文件有两种：

- `.github/copilot-instructions.md`：每次向 Copilot 提交存储库请求时都会发送此文件，应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，例如 TypeScript、Astro 文件、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时会使用存储库级说明以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。Copilot 也支持 AGENTS.md 和 CLAUDE.md 等规范中的说明文件。

### 管理说明文件的最佳实践

- 将 `copilot-instructions.md` 限定为项目级指南，例如项目结构、技术栈和全局编码标准。
- 使用 `*.instructions.md` 为单元测试、Astro 组件和数据层等文件类型或任务提供专属指南。
- 使用自然语言清晰说明要求，并提供示例。

说明文件没有唯一的编写方式，需要通过实践找到适合项目的做法。

> [!TIP]
> 项目可以为多种任务提供说明，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。Copilot 也可以帮助生成说明文件：VS Code 中的 **Configure Chat → Generate Agent Instructions** 和 Copilot CLI 中的 `/init` 都提供相应入口。还可以在 [awesome-copilot][awesome-copilot] 中查找模板和示例。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

## 查看项目中的自定义说明文件

打开存储库附带的 `.github/copilot-instructions.md` 和 `.github/instructions/*.instructions.md` 文件，了解项目如何组织说明：

1. 打开 `.github/copilot-instructions.md`，查看项目简介、**Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure**。特别留意 **Code standards** 中的 **GitHub Actions Workflows** 指南。
2. 浏览 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层和测试等说明。
3. 打开 `.github/instructions/unit-tests.instructions.md`，查看 `applyTo` frontmatter。它设置了相对于存储库根目录的 glob 模式；例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件适用这些说明。
4. 查看单元测试说明中的要求。
5. 打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾。注意其中指向其他说明文件和现有示例的链接。这种方式可将大型说明拆分为可复用的小文件。（路径相对于当前说明文件，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求添加代码文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 为更改创建分支

1. 返回上一练习中创建的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace 并打开 Copilot Chat 面板，然后选择 **New Chat** 开始新的对话。每项练习使用的模式和模型可能不同，相关步骤会在适用时说明。

先在现有说明文件下发送提示，随后更新说明并重新发送相同提示，以便观察效果。

1. 关闭之前练习中打开的编辑器选项卡，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应放置的位置。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以便 Copilot 编辑文件。

   ![显示聊天视图智能体选择器的屏幕截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问 helper，返回所有出版商及其名称和 ID。应用文件更改。
   ```

5. Copilot 会探索项目并应用代码更改，通常包括 helper 文件和测试。
6. 查看生成的 helper：它应采用类型化函数，以 `db` 客户端为第一个参数并返回类型化的 publishers 数组。这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层约定。
7. 注意生成的代码**没有** TSDoc 文档注释或文件级注释头。

> [!CAUTION]
> Copilot 具有非确定性，因此即使没有要求也可能添加文档注释。这并不影响练习重点：更新说明后，结果会更稳定地遵循标准。

## 添加新的存储库标准

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意其中尚未说明添加代码文档的要求。
3. 在现有标准下方添加以下 Markdown，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - 每个导出函数都应包含 TSDoc 注释，说明其用途、参数和返回值。
   - 在导入语句或其他代码之前，添加注释块说明文件用途。
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 存储库级 `.github/copilot-instructions.md` 适合记录文档注释等项目级标准；`*.instructions.md` 则适用于特定语言、文件类型或任务。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清空上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送与之前相同的提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问 helper，返回所有出版商及其名称和 ID。应用文件更改。
   ```

4. 查看生成的文件是否包含类似以下的文件注释头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的出版商数据访问 helper。
    * 提供从数据库获取出版商信息的函数。
    */
   ```

5. 查看函数是否包含类似以下的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有出版商的 ID 和名称列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为出版商对象数组的 Promise。
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用到下一练习要用的代码中。

## 提交说明并推送分支

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
2. 运行 `git status`，确认说明更新和 helper 更改都已存在。
3. 暂存并提交说明更新以及 Copilot 为 helper 基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更新（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并 pull request

分支推送后，可以手动创建 pull request，也可以让已连接到项目的 GitHub 工具帮助 Copilot 查找 issue、创建 PR 并将其合并。

> [!NOTE]
> 这是本课程中首次让 Copilot 在 GitHub 上代你执行操作。项目模板已配置 GitHub 工具，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示完成授权。后续课程会介绍模型上下文协议 (MCP) 的工作方式。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器中已选中 **Agent**。
3. 请 Copilot 查找与更新说明文件有关的 issue，并基于当前分支创建 PR，说明其中包含说明更新和 publishers helper 基础：

   ```
   查找与更新说明文件有关的 issue。基于当前分支创建 PR，注明此 PR 会关闭该 issue，并说明我们还为后续筛选工作添加了 publishers helper 基础。
   ```

4. Copilot 会查找 issue 并创建 PR。
5. Copilot 提示允许其代你执行 GitHub 操作时，先审查命令，然后按需选择 **Allow**。
6. PR 创建后，要求 Copilot 合并 PR 并将本地分支切回 main：

   ```
   将 PR 合并到 main。然后在本地切回 main 并拉取最新代码，确保本地代码保持最新。
   ```

7. Copilot 提示允许其执行 GitHub 操作和 shell 命令时，先审查命令，再按需选择 **Allow**。

你已借助 GitHub Copilot 创建并合并 pull request。

## 总结和后续步骤

你已了解 Copilot 如何使用说明文件，并在 VS Code 中：

- 发送代码生成提示，观察 Copilot 如何根据**现有**说明生成代码。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 重新发送相同提示，观察生成代码如何遵循新标准。
- 将说明更新和 helper 基础提交到 `main`，供下一练习使用。

接下来在[智能体模式][next-lesson]中使用这些说明，跨代码库添加功能。

## 资源

- [自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更佳 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
