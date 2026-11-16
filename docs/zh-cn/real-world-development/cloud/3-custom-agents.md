---
title: "练习 3 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的 AI 助手。通过在存储库的 `.github/agents` 文件夹中创建 Markdown 文件来定义代理，可以为 Copilot 提供聚焦的说明、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理成可重复使用的代理，例如确保符合 [WCAG][wcag] 标准的无障碍代理、遵循安全编码实践的安全代理，或维护一致测试模式的测试代理。

可以通过项目 `.github/agents` 文件夹中的 Markdown 文件定义自定义代理，也可以在 `~/.copilot/agents` 中全局定义。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description` 字段，后面是定义代理行为、专业知识和说明的 Markdown 提示。

### 自定义代理与代理技能的比较

自定义代理与[代理技能][agent-skills-concept]之间存在一些逻辑上的重叠。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理**负责工作，而**技能**是工具。

自定义代理拥有自己的上下文窗口，并负责协调技能（甚至其他代理）来完成工作。在本实验中，无障碍自定义代理会依据无障碍指南审核并更新网站；在执行过程中，它可以调用诸如拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一的“正确”方式。与 AI 的其他用法一样，需要不断测试和迭代，找到适合自己环境和场景的方法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

使用自定义代理时，你将探索：

- 如何定义自定义代理；
- 如何向自定义代理分配任务。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前深色主题的文本与背景对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

由于无障碍至关重要，你希望尽快完成此功能。因此将使用自定义代理来生成相关功能。

## 查看无障碍自定义代理

已经为你创建了一个无障碍自定义代理。现在先查看其内容，了解它将如何指导 Copilot。

返回 codespace，然后查看无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> `name` 和 `description` frontmatter 是自定义代理的必需项。

3. 接着浏览后续部分，了解其中介绍的内容：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见陷阱和错误。

## 创建并分配 issue

任务控制中心是管理环境中所有代理的中心位置。你可以将任务分配给 Copilot cloud agent、监控任务，甚至引导正在进行的会话。现在开始将创建高对比度模式的任务分配给 Copilot。

1. 打开存储库。
2. 选择**议题**选项卡。
3. 选择**新建议题**，打开新建议题对话框。
4. 选择**空白议题**，创建空白议题。
5. 将**标题**设置为 `为网站添加高对比度模式`。
6. 将**说明**设置为：

    ```plaintext
    我们需要为网站添加高对比度模式。应提供一个可由用户设置的高对比度切换开关，并将设置保存在浏览器的 local storage 中。
    ```

7. 选择**创建**，创建议题。
8. 在右侧选择**分配给 Copilot**，打开分配对话框。
9. 从自定义代理列表中选择**无障碍代理**。

    ![云代理分配界面截图，突出显示自定义代理和 Accessibility](../../../_images/ex5-select-custom-agent.png)

10. 选择**分配**。
11. Copilot 就会在后台开始处理任务！

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]。自定义代理是针对特定任务和领域定制的 AI 助手。借助自定义代理，可以将团队的专业知识和标准整理成可重复使用的说明，帮助 Copilot 更有效地完成特定类型的工作。

本课介绍了以下概念：

- 如何定义自定义代理；
- 如何向自定义代理分配任务。

Copilot 正在实现高对比度模式，现在我们可以转而从任务控制中心[监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [为在组织中使用自定义代理做准备][org-custom-agents]
- [为在企业中使用自定义代理做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
