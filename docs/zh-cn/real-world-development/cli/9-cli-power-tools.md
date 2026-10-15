---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和委派到云端的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他 CLI 工具一样，GitHub Copilot CLI 包含许多斜杠命令，可用于访问高级功能、内部信息和配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr`。本课将介绍如何：

- 使用 `/context` 和 `/compact` 探索 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解如何通过 `/share` 导出或共享会话。
- 探索用于并行工作、工作树和委派给云智能体的可选命令。

## 场景

CLI 核心工作流已完成。现在了解管理上下文、切换模型、共享会话，以及可选地委派工作给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理较大或复杂任务时，可能会遇到模型上下文窗口的容量限制。Copilot CLI 会在需要时自动压缩对话，也可以用斜杠命令查看或手动压缩：

1. 返回 Codespace。如果 Copilot CLI 未运行，请在存储库根目录启动它。
2. 输入 `/context`，查看模型、当前令牌使用量，以及上下文在系统说明、工具、消息和可用空间中的分配。
3. 输入 `/compact` 压缩对话。
4. 再次输入 `/context` 并比较结果。如果对话尚短，变化可能不明显。

> [!NOTE]
> Copilot CLI 会在上下文窗口即将填满时自动压缩。使用 `/compact` 可自行决定压缩时机；如果要切换到无关任务并开始全新对话，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发人员也有不同偏好。输入 `/model` 查看可用模型和使用信息；可以保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

通过 `/share` 可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布为 GitHub gist。输入 `/help` 查看当前版本支持的选项。共享前请审查导出内容，其中可能包含提示、回复和项目详情。发布链接或 gist 是可选操作，不要公开团队不打算分享的存储库或对话内容。

## 可选：扩展并行任务或委派工作

核心研讨会已经完成。Copilot CLI 还提供以下可选命令：

- `/fleet` 可将独立子任务交由子智能体并行处理。
- `/worktree` 可为单独任务创建隔离的 Git 工作树。
- `/delegate` 可将任务委派给 Copilot 云智能体，异步处理并可能创建 pull request。

这些命令可能会创建其他工作树或远程任务，因此请先从范围明确的新任务开始，并通过常规工作流审查结果。若要进一步了解异步智能体，请继续学习[云智能体研讨会][cloud-workshop]。

## 总结和后续步骤

使用 GitHub Copilot CLI 中的斜杠命令可以进行配置、共享会话并查看后台活动。本课中，你：

- 使用 `/context` 和 `/compact` 探索 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解如何通过 `/share` 导出或共享会话。
- 探索用于并行工作、工作树和委派给云智能体的可选命令。

Copilot CLI 还有更多斜杠命令可供探索。接下来[回顾所学内容][next-lesson]并了解后续学习方式。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
