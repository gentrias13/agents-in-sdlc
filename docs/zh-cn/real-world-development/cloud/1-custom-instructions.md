---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能够访问这些内容。[说明文件][instruction-files]可提供相关指引，帮助 Copilot 理解你希望它做什么，以及应该如何完成。

本练习将介绍如何：

- 通过存储库自定义说明和按路径限定范围的说明文件，为 Copilot 提供项目背景、编码指南和文档标准。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，这里不会运行前后对比提示。Copilot 云智能体会在 GitHub issue 上异步工作，因此较难实时并排展示其影响。在本环境后续课程审查云智能体创建的 pull request 时，可以看到说明文件如何影响结果。

## 说明文件

### 场景

Tailspin Toys 和其他成熟开发团队一样，有一套开发实践指南和要求：

- 数据层始终需要单元测试。
- UI 应使用深色模式，并具有现代感。
- 代码中应使用 TSDoc 文档注释。
- 每个文件开头都应有一段注释，说明文件用途。

使用说明文件可确保 Copilot 获取正确的信息，并按照这些实践完成任务。

### 自定义说明

自定义说明可向 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求，从而生成更相关的建议和代码片段。你可以指定偏好的编码约定、库，甚至希望添加的注释类型。说明可以覆盖整个存储库，也可以只针对特定文件类型或任务。

说明文件有两种：

- `.github/copilot-instructions.md`：每次向 Copilot 提交存储库请求时都会发送此文件。它应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概览、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，例如 TypeScript、Astro 文件、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级说明以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他规范中的说明文件，包括 AGENTS.md 和 CLAUDE.md。

[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses

### 管理说明文件的最佳实践

完整讨论说明文件的创建方法超出了本课程范围，但示例项目展示了常见做法：

- 将 `copilot-instructions.md` 限定为项目级指南，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为单元测试、Astro 组件或数据层等文件类型或任务提供具体指南。
- 使用自然语言，清晰说明要求，并提供代码示例。

说明文件没有唯一的编写方式，就像 AI 的使用方式也不止一种。通过实践，可以找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的说明文件。查看本项目时，可能会发现它为多种任务提供了说明，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也能帮助生成说明文件。不同界面中的入口有所区别，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。所在界面的课程会在适用时介绍相应操作。
>
> 若要查找模板或起点，可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义说明文件

花些时间阅读存储库附带的说明文件：其中有一个核心 `copilot-instructions.md`，还有一组用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。请特别留意 **Code standards** 中的 **GitHub Actions Workflows** 指南，它适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览内容。注意其中包含 Astro 文件、Drizzle 数据层、测试等相关说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它设置了相对于存储库根目录的 glob 模式，用于确定说明适用的文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件适用这些说明。
5. 查看该文件中有关创建单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这种方式可以将较大的说明拆分为更小、可复用的文件，并链接到可供参考的示例。（这些路径相对于说明文件所在目录，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但尚未要求添加代码文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的存储库标准

接下来是本练习中唯一需要编辑的内容：添加项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。稍后分配 issue 时，云智能体会遵循这些规则。

编辑前先创建一个分支（云智能体会从 issue 目标分支读取说明，因此你需要最终将更改合并到对应分支）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

存储库级的 `.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。更新存储库编码标准可以改善代码建议：

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。该部分记录了项目的编码标准，但尚未包含代码文档要求，这就是生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - 每个导出函数都应包含 TSDoc 注释，说明其用途、参数和返回值。
   - 在导入语句或其他代码之前，添加注释块说明文件用途。
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级 `.github/copilot-instructions.md` 中设置全局指南，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供说明。你添加的文档注释规则属于项目级标准，适合放在存储库级文件中。

## 提交、推送并合并说明更新

云智能体会从 issue 所在分支读取说明。下一练习中将 issue 分配给 Copilot 时，它会从 `main` 分支开始工作，因此必须先将说明更改合并到 `main`：

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 指向 `main` 的 pull request，并将其合并。之后在下一练习中分配 issue 时，云智能体就会读取这些说明。

> [!TIP]
> 如果希望在本次研讨会中直接使用 `main`，可以跳过分支操作并直接提交到 `main`。这里使用分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何使用本项目中的说明文件，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。在本环境后续课程审查云智能体创建的 pull request 时，可以看到这些标准的作用。

接下来[将第一个 issue 分配给 Copilot 云智能体][next-lesson]。

## 资源

- [自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更佳 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
