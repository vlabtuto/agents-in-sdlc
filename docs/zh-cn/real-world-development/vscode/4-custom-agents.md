---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]让你可以创建专门的 AI 助手，针对开发工作流中的特定任务或领域进行定制。你可以通过存储库 `.github/agents` 文件夹中的 Markdown 文件定义代理，为 Copilot 提供聚焦的说明、最佳实践、编码模式和领域知识，从而引导它更有效地完成特定类型的工作。团队可以将专业知识编写成可复用的代理，例如确保符合 [WCAG][wcag] 要求的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局目录 `~/.copilot/agents` 中定义。每个文件都包含 YAML frontmatter，其中至少要有 `name` 和 `description`，后面是定义代理行为、专业知识和说明的 Markdown 提示。

### 自定义代理与代理技能的比较

自定义代理和[代理技能][agent-skills-concept]之间存在一些逻辑上的重叠。两者主要通过 Markdown 文件定义，用于指导 AI 如何执行操作。最简单的区分方式是：**自定义代理**是执行者，**技能**是工具。

自定义代理拥有独立的上下文窗口，能够编排技能（甚至其他代理）来完成任务。在本实验中，无障碍自定义代理会依据无障碍指南审查并更新网站；在此过程中，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一的“正确”方式。与 AI 的其他用法一样，需要针对你的环境和场景进行测试和迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好限制。近期用户反馈指出，部分用户觉得当前深色主题难以阅读，因为文本与背景颜色的对比度不足。为解决此无障碍问题，设计团队要求实现一种可以由用户开关的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。你将使用自定义代理来生成相关功能。

在本练习中，你将：

- 查看现有的无障碍自定义代理；
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 查看无障碍自定义代理

我们已经为你创建了一个无障碍自定义代理。现在查看其内容，了解它将如何引导 Copilot。

返回 codespace，打开终端，然后从 `main` 创建新分支并切换到该分支，以单独开展无障碍相关工作（练习 3 的筛选 PR 将保留在单独的分支上）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> `name` 和 `description` 是自定义代理必须包含的 frontmatter 字段。

3. 接着浏览并查看以下部分：
   - 生成无障碍网站代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见问题和错误列表。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉菜单中显示 `.github/agents` 中定义的所有自定义代理。你可以选择自定义代理，让聊天会话使用该代理的说明和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始一个干净的对话。每个练习对模式和模型的选择可能不同，相关步骤会在需要时说明。

1. 如果 Chat 视图的代理下拉菜单中尚未选中 **Agent**，请先选择它。

   ![Chat 视图中的代理选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择聊天视图底部的代理下拉菜单（其中显示当前代理，默认为 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

    ```plaintext
    为网站添加高对比度模式。应提供一个高对比度切换开关，供用户设置，并使用浏览器的 local storage 在页面重新加载后保留此设置。
    ```

5. Copilot Chat 会开始工作：它将探索代码库、提出更改建议，并将编辑应用到项目文件。每项编辑都会在聊天中内嵌显示，其中包含文件路径和可供审查的差异。
6. 随着编辑逐步应用，**Files changed** 指示器会更新，让你查看代理修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库中的文件，例如 Astro 组件、CSS 和相关测试。

下一课将介绍如何审查并引导这项正在进行的工作。

## 总结和下一步

本课介绍了 [GitHub Copilot 自定义代理][custom-agents]，它们是针对特定任务和领域定制的 AI 助手。通过自定义代理，可以将团队的专业知识和标准编写成可复用的代理，更有效地引导 Copilot 完成特定类型的工作。

你学习了以下内容：

- 自定义代理的定义方式；
- 如何在 Copilot Chat 代理模式中使用自定义代理。

接下来，你将[监视并引导代理工作][next-lesson]，在工作进行时审查更改，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监视和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
