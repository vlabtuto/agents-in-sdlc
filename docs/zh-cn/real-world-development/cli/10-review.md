---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的完整工作流中使用了 GitHub Copilot CLI。你：

- 准备了 Codespace，安装了 Copilot CLI，探索了项目，并找到了预先创建的筛选问题。
- 添加了星级评分，通过端口转发在浏览器中检查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选问题入手，在 Plan 模式中制定方案，在 Autopilot 模式中构建功能，并在 Interactive 模式中审查结果。
- 使用自定义指令引导代理，然后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加了 Playwright 模型上下文协议（MCP）服务器，并通过真实浏览器探索筛选功能。
- 创建并选择了 QA 自定义代理，用于评估需求、覆盖率、技能运行结果和浏览器证据。
- 审查了完整的筛选功能更改，并授权 Agent Merge 合并筛选 PR。
- 探索了用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 已交付的内容

本次研讨会包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 分支创建自己的分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating`，并明确显示未评分状态。
2. **筛选和质量工作流：**实现筛选功能，更新说明并将其应用于该功能，自定义 `quality-checks` 报告，创建 QA 配置，并加入相关测试。

从规划筛选功能到创建 PR，你始终使用同一段对话和同一个分支。为了简化本次研讨会，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 使用 Copilot 探索浏览器。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按你新设定的格式报告结果。QA 在 PR 创建前汇总了这些结果，并审查了需求和测试覆盖率。

新增测试应当弥补实际存在的缺口；QA 检查后未新增测试也可能是合理的。授权合并前，请审查代码和验证证据；更改之后，请更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本次研讨会中，你更新了指令、自定义了技能、创建了 QA 配置，并配置了 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。指令用于设定标准，技能用于描述可重复的任务，自定义代理用于定义专业角色，MCP 服务器则用于连接外部工具。请检查实际更改和工具结果，而不只是代理的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前思考方案，使用 **Interactive** 跟进范围明确的更改，使用 **Autopilot** 处理定义清晰的任务。日常编辑可以选择速度更快的模型；复杂工作则选择能力更强的模型。

上下文和基础设施同样重要。清晰说明要构建**什么**、**为什么**以及**如何构建**，会显著影响输出。

## 更多探索

你已经完成核心工作流。还可以了解一些 Copilot CLI 功能：

- `/review`：请求代码审查代理分析更改。
- `/rubber-duck`：讨论问题并获得另一种观点。
- `/fleet`：并行协调相互独立的子任务。
- `/worktree`：隔离单独的任务。
- `/delegate`：将任务发送给 Copilot 云端代理。

## 后续步骤

提升工具使用能力的最佳方式就是持续使用它！可以用它处理生产代码、兴趣项目，或者多年来一直想构建却尚未着手的小应用。与团队分享经验，也向团队成员学习。别忘了继续探索文档。

如果想从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot 应用研讨会][app]或 [Copilot 云端代理研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
