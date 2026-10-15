---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "查看存储库说明，添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，就要确保 Copilot 可以访问相关指导。[说明文件][instruction-files]不仅描述希望生成什么代码，也能说明代码应如何组织。本课将查看 Copilot 使用的说明、添加文档标准并将其应用于筛选功能。

本课将介绍如何：

- 查看存储库说明和按路径限定范围的说明文件如何传递给智能体。
- 更新说明文件，确保遵循编码标准。
- 了解说明文件对代码的影响。

## 场景

Tailspin Toys 的开发实践要求注释说明意图和不明显的决定，而不是复述代码；`db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 记录用途、参数和返回值（包括可注入的 `db` 参数）；可复用 Astro 组件应说明 `Props` 契约；代码变化时应同步更新注释；并保留现有格式和 lint 指南。

## 说明文件

存储库级 `.github/copilot-instructions.md` 会在每次请求中发送给 Copilot；`.github/instructions/*.instructions.md` 则适用于特定语言、文件类型或任务。不同环境对其他说明格式的支持可能不同，使用前请查阅[自定义说明支持参考][custom-instructions-support]。

1. 返回 Codespace，在编辑器（不是终端）中打开 `.github/copilot-instructions.md`，查看项目简介和适用于 Copilot 所有交互的编码指南。
2. 浏览 `.github/instructions`，查看 Astro、Drizzle 数据层、测试等说明。
3. 打开 `.github/instructions/unit-tests.instructions.md` 并查看 `applyTo` 字段，了解它如何限定说明的适用文件。
4. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他说明文件和项目文件的链接。

## 更新说明以符合团队指南

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分（通常在第 35 行附近）。
2. 在最后一个项目符号后添加以下规则：

   ```markdown
   - 所有新增 TypeScript 代码都应包含 TSDoc 注释，以提供文档说明。
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库说明。保存更新后，退出并恢复筛选功能对话，让 Copilot 在保留功能上下文的同时加载新说明。

1. 如果筛选对话仍处于打开状态，输入 `/exit`。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，选择筛选功能对话并确认当前检出的是 `game-filters-cli` 分支。
3. 要求 Copilot 应用更新后的指导：

   ```plaintext
   我们刚刚更新了说明和代码指南。请更新你生成的代码，使其符合这些指南。
   ```

4. 输入 `/diff` 并审查更改的 TypeScript 文件。确认新增的 TSDoc 注释能准确说明代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何读取说明文件，并将新标准应用于筛选功能。具体来说，你：

- 查看了存储库说明和按路径限定范围的说明文件如何传递给智能体。
- 更新了说明文件，以确保遵循编码标准。
- 了解了说明文件对代码的影响。

接下来[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试能够一致地运行。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件合集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
