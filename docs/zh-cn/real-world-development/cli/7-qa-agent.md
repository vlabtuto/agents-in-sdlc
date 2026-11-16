---
title: "第 7 课 - 创建并使用 QA 代理"
description: "创建 QA 自定义代理，将 issue 需求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已经使用 `quality-checks` 技能运行自动检查，并使用 Playwright MCP 在浏览器中查看筛选体验。现在将这些能力整合到一个自定义代理中，并为它定义清晰的 QA 流程。

本课将带你：

- 了解自定义代理如何配合指令、技能和 MCP 工具工作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 代理，并根据筛选功能的 issue 审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）之前，始终一致地审查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义代理可以协调 QA 流程，并提供可复用的报告。

## 什么是自定义代理？

自定义代理是在 Markdown 配置文件中定义的 Copilot 专用版本。配置文件说明代理的用途、指令和可用工具。在本研讨会中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用的各项自定义功能各有用途：存储库指令说明团队标准；`quality-checks` 技能封装可重复运行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则告诉 Copilot 如何利用这些能力评估需求并报告发现。它不会取代这些功能，也不需要另开对话。

## 创建 QA 配置文件

创建功能 PR 之前，让 Copilot 创建一个可复用的 QA 配置文件。该配置文件将定义 QA 要执行的检查以及必须遵守的边界。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 让默认代理创建新的自定义代理：

   ```plaintext
   在 .github/agents/qa.agent.md 中创建一个名为 QA 的自定义代理。它应根据 issue 和已达成一致的需求检查功能，遵循存储库指令，运行 quality-checks 技能，使用 Playwright MCP 验证行为，并在测试覆盖不足时添加测试。

   要求它将每项需求标记为通过、失败或受阻，并提供支持证据。修改实现代码前必须征求许可，并且不得提交变更或创建拉取请求。

   现在只创建配置文件，供我审查。
   ```

## 检查配置文件

使用新代理前，先审查其配置文件，确认 Copilot 已按预期设置 QA 工作流和边界。这样可以避免配置不完整或范围过大的代理在你只想验证功能时修改功能本身。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 为可选项，但填写后代理会有清晰的显示名称。
3. 阅读配置文件中的指令，确认 QA 会从需求出发、遵循存储库指令、运行 `quality-checks` 技能，并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会征求许可，并且不会提交变更或创建拉取请求。
5. 如果生成的配置文件遗漏了上述职责或边界，请让默认代理修改后再继续。

## 根据 issue 运行 QA

Copilot CLI 会在对话开始时加载项目代理。创建配置文件后，恢复同一个筛选对话，然后选择 QA，这样它就能使用上下文中的 issue 和规划决定。

1. 使用以下提示启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于你刚创建了这个代理，它可能不会显示在可用代理列表中。它已经存在，可以使用上述命令将其激活。

2. 使用以下提示，让 QA 代理审查此功能：

   ```plaintext
   根据 issue 和计划中的决定审查筛选功能。它是否已准备好创建 PR？
   ```

3. QA 代理将开始工作！
4. 完成后，阅读它提供的报告。

## 总结和后续步骤

你添加了一个可复用的专业角色，并审查了它的工作。本课中，你：

- 了解了自定义代理如何配合指令、技能和 MCP 工具工作。
- 创建并检查了可复用的质量保证配置文件。
- 选择了 QA 代理，并根据筛选功能的 issue 审查了它的发现。

现在，你已准备好实现代码、技能更新、QA 配置文件、测试和验证报告，可以开始审查。接下来，你将[在功能 PR 中整合这些内容，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
