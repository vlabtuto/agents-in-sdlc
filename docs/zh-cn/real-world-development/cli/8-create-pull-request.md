---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、自定义指令更新、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能的实现、自定义指令更新、技能更新、质量保障（QA）配置文件和测试都已保存在同一个分支中。现在可以一起审查这些内容并创建拉取请求（PR）。你已经亲自合并了星级评分 PR；这次将由 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常，我们会将功能、自定义指令更新、技能更新和 QA 代理拆分为几个独立的 PR。为了简化本次研讨会，我们在一次对话和一个分支中完成了整个筛选和质量工作流，并将所有更改放入这个 PR。

在本课中，你将：

- 了解 Agent Merge 及其如何自动处理合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 仅在审查完成后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选工作流中，你都使用 Copilot 规划、实现并验证功能。现在，Tailspin Toys 希望自动处理剩余的 PR 工作，同时让开发者继续掌控合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动完成合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的问题——修复未通过的持续集成（CI）检查、回复审查意见，并在需要时变基——然后在存储库允许的情况下启用 GitHub 自动合并。

到目前为止，你都是自行选择 **Merge pull request**。Agent Merge 可以代你处理这项工作。启用 Agent Merge 前，请先审查更改并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码全部创建后，一起审查更改、创建 PR，然后让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开代理对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监视 PR 的持续集成流程。所有检查通过后，Copilot 会执行合并。
7. 确认看到 Copilot 发出的类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限要求。如果流程受阻，请查看报告的原因，并在存储库允许的情况下完成经审查的手动合并。

## 总结和后续步骤

你已自动处理开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。你：

- 了解了 Agent Merge 及其如何自动处理合并流程。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查完成后启用了 Agent Merge，并确认 PR 已合并。

接下来，你将[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，了解上下文、模型、共享和可选的云端委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
