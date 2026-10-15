---
title: "第 2 课 - 添加星级评分：快速上手"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中审查更改，并将其作为第一个 pull request 合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并进行初步对话后，现在为项目做第一个更改。游戏数据中已有星级评分，但主页上的游戏卡片尚未显示它。让智能体显示评分、审查更改，并将其作为第一个 pull request 合并。

本课将介绍如何：

- 在功能分支上开始专注于某项功能的 Copilot 对话。
- 让智能体为项目进行小幅更改。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认更改。
- 创建并合并第一个 pull request。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，游戏详情页已经显示该评分，但主页游戏卡片只显示标题、类别、出版商和说明。接下来让智能体在每张卡片上显示已有评分，这是适合首次会话的小型独立更改。

## 对话的组成

**对话**是你使用 Copilot CLI 完成任务的空间。与 Copilot app 不同，常规 CLI 对话使用终端中当前检出的存储库和 Git 分支，不会创建专用工作树。保存的对话可供之后继续使用，文件和分支仍是磁盘上的普通 Git 状态。

对话中会显示你的提示和智能体回复、智能体探索和编辑文件时的工具活动，以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

1. 返回 Codespace。
2. 如果终端未打开，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未启动，运行：

   ```bash
   copilot --yolo
   ```

4. 输入 `/new` 并按 <kbd>Enter</kbd>，确保开始新会话。
5. 输入以下提示：

   ```plaintext
   在列表页的游戏卡片中显示每款游戏的 `starRating`（满分 5 分）。如果评分为 null，则显示“暂无评分”。保持卡片布局不变，添加测试并运行相关检查。
   ```

Copilot 会探索项目，找到显示游戏详情的文件并生成所需代码。现在已使用 Copilot CLI 添加功能。

## 审查差异

1. 输入 `/diff` 并检查所有更改文件。
2. 确认游戏卡片会在评分存在时显示数值；当 `starRating` 为 `null` 时显示“暂无评分”。
3. 确认测试覆盖这两种状态。
4. 查看 Copilot 运行的检查结果，并要求它修复失败项。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> Copilot 和其他生成式 AI 工具一样，具有非确定性，因此具体代码可能不同。请审查行为，不要期待实现与示例完全相同。

## 验证更改

让 Copilot 启动网站，以便在 Codespaces 转发的浏览器中检查更新后的用户界面 (UI)：

1. 要求 Copilot 启动应用：

   ```plaintext
   启动应用，以便我在浏览器中检查星级评分更改。告诉我 URL，并让服务器继续运行。
   ```

2. Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了五分制评分。
4. 返回 Copilot 并要求它停止启动的服务器：

   ```plaintext
   停止你启动的开发服务器。
   ```

## 创建并合并第一个 pull request

1. 要求默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 会推送现有提交、创建 PR 并显示 URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），并选择 Copilot CLI 显示的 URL 打开 PR。
4. 审查更改文件和检查结果。
5. 确认无误后选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜你！你已使用 GitHub Copilot CLI 发布第一个更改。具体来说，你：

- 在功能分支上开始了专注于某项功能的 Copilot 对话。
- 引导智能体小幅修改游戏卡片。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认星级评分。
- 创建并合并了第一个 pull request。

接下来从筛选 issue 开始，使用 [Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
