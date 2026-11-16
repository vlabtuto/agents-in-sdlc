---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审核工作

在整个实验中，你与 GitHub Copilot 一起完成了多项任务，重点是改善用户体验和添加功能。你让 Copilot 为代码添加文档、为设计团队构建相关游戏功能以供迭代，并实现包括高对比度和浅色模式切换开关在内的无障碍功能。现在我们来查看代码更改，并在必要时向 Copilot 提供反馈，帮助它改进工作。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审核生成的代码，并遵循常规 DevOps 流程。因此，在让团队其他成员开始审核之前，我们先查看 GitHub Copilot 提出的文档、相关游戏功能和无障碍功能方案。

## 安全性与 GitHub Copilot cloud agent

由于 Copilot cloud agent 会异步且无人监督地执行任务，因此设置了一些安全限制，以确保工作过程安全。这些限制包括：

- Copilot 仅对存储库具有读取权限，对用于编写代码的分支具有写入权限。
- Cloud agent 在 GitHub Actions 中运行，并创建一个独立的临时环境来开展工作。
- 运行任何 GitHub Actions 工作流都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括对 MCP 服务器的访问。

## 查看生成的文档

先查看 GitHub Copilot cloud agent 生成的第一个拉取请求（PR），即为代码添加文档。此操作使用 GitHub.com 的标准 PR 界面。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为默认情况下 Copilot 对外部资源（包括外部 MCP 服务器）的访问受到限制。你也可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择**拉取请求**，打开拉取请求列表。
3. 打开标题类似 **添加缺失的文档** 或者更完整的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟，你可以稍作休息，或回顾目前学到的内容。

4. PR 准备就绪后，选择**已更改文件**选项卡并查看更改。

    ![已更改文件选项卡](../../../_images/shared-pr-files-changed.png)

5. 浏览更新后的代码，其中包含新建的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和注释头。这些要求来自你在练习 2 开始时查看的自定义说明文件。

6. 审核更新并确认没有问题后，返回**对话**选项卡并向下滚动。
7. 你应该会看到有工作流正在等待批准的提示。
8. 如果有工作流等待批准，请选择**批准并运行工作流**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 你应该会在拉取请求的检查部分看到工作流排队运行。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向过程。你可以像在拉取请求中提及团队其他成员一样，在评论或代码行内评论中提及 Copilot。Copilot 会看到这些评论，并启动另一个会话来处理请求。由于结果具有非确定性，我们无法提供固定的请求文本。可以考虑要求 Copilot 更新以下内容：

- 在每个代码文件顶部添加简短说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建一个 README，介绍 Astro 应用的结构。

1. 添加评论，请求修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以使用上面的任一想法，也可以提出其他希望在代码库中看到的文档改进建议。
2. 选择**查看会话**，查看 Copilot 执行任务。留意 Copilot 如何启动新会话进行更新。
3. 选择**返回拉取请求**。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，你应该会在拉取请求中看到新的提交。
5. 选择**已更改文件**选项卡，审核更改。

可以继续迭代，直到满意为止。满意后，可以将 PR 从草稿转换为准备审核状态，然后合并到主分支。

![将 PR 转换为准备审核状态](../../../_images/ex7-ready-for-review.png)

## 审核相关游戏功能

现在回到 Copilot 为解决“在游戏详情页显示相关游戏”问题而生成的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择**拉取请求**选项卡。
3. 选择标题类似 **在游戏详情页显示相关游戏** 或者更完整的 PR。
4. 选择**已更改文件**选项卡，查看生成的代码。
5. 审核更新并确认没有问题后，返回**对话**选项卡并向下滚动。
6. 你应该会看到有工作流正在等待批准的提示。
7. 如果有工作流等待批准，请选择**批准并运行工作流**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 你应该会在拉取请求的检查部分看到工作流排队运行。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。前往 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经构建了相关游戏功能！和之前一样，你可以通过迭代的方式与 Copilot cloud agent 协作并请求更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或确保添加注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义说明！）。和之前一样，可以在**对话**选项卡中添加评论来提出请求，Copilot 会看到评论并启动新会话。

## 审核无障碍功能

最后，查看使用无障碍自定义代理实现的功能。此 PR 应该同时包含你在练习 3 中分配的高对比度模式，以及在练习 4 的任务控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择**拉取请求**选项卡。
3. 选择标题类似 **为网站添加高对比度模式** 或者更完整的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择**已更改文件**选项卡，查看生成的代码。
5. 查看实现，重点留意：
   - 用于切换模式的 UI 组件；
   - 用于保存用户偏好的 local storage；
   - 高对比度模式和浅色模式对应的 CSS 或样式更改；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审核更新并确认没有问题后，返回**对话**选项卡并向下滚动。
7. 你应该会看到有工作流正在等待批准的提示。
8. 如果有工作流等待批准，请选择**批准并运行工作流**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 你应该会在拉取请求的检查部分看到工作流排队运行。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
10. **可选：**你可以在 Codespace 中切换到此分支，手动测试无障碍功能。前往 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用程序，并在浏览器中测试高对比度和浅色模式切换开关，确认功能正常且在重新加载页面后仍会保留设置。

留意无障碍自定义代理如何引导 Copilot 按照无障碍最佳实践实现这些功能。如果发现任何无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 请求更新。

## 可选练习 — 继续委派任务

Cloud agent 最适合处理实际积压工作，让你可以把注意力转向其他事情。可以向存储库提交几个 issue 并分配给 Copilot，养成这种协作习惯。以下是一些想法：

- 在游戏详情页中创建支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 实践课程。在这些课程中，你：

- **检查了此存储库提供的自定义说明文件**，以便之后了解它们对 cloud agent 输出的影响；
- **向 Copilot cloud agent 分配了 issue**，并观察它如何设置环境、制定计划并异步执行任务；
- **创建并使用了自定义代理**来实现无障碍功能，包括高对比度和浅色模式切换开关；
- **使用 Copilot 代理页面作为任务控制中心**，监控并引导正在进行的无障碍会话；
- **审核并迭代了 cloud agent 的拉取请求**，通过标记 `@copilot` 请求更改并批准工作流。

## 复习和后续步骤

你已完成 Cloud agent 实践课程。如果想继续探索，其他课程模块可以补充你在这里练习的内容：

- 🖥️ **[VS Code 实践模块](../../vscode/)** — 在 IDE 中直接探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 实践模块](../../cli/)** — 在终端中通过 Plan 和 Autopilot 模式、代理技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接功能，交付两项经过审核的更改。

在自己的存储库中，可以尝试以下后续练习：

- 将重构或测试覆盖率 issue 分配给 cloud agent。
- 为其他领域创建自定义代理。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
