---
title: "第 3 课 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确定实施方案，使用 Autopilot 根据 issue 实现筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先前为项目添加了一项小功能。但较大的变更需要更完善的流程。GitHub Copilot CLI 能配合组织现有的工作流，帮助我们以正确的方式构建正确的内容。这是多节课程中的第一节，你将按照典型的代理驱动开发流程，先从 issue 实现新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 继续完成此功能工作流时，你会沿用同一对话和分支。通常，不同文件类型的工作会使用不同的分支或 PR，但为了让你专注于核心概念，我们会简化流程。

本课将带你：

- 从 GitHub issue 开始新的 Copilot 对话。
- 在 Plan 模式下明确需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续完成此功能时，你还将更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理，并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩大，访问者需要按类别和发行商筛选游戏。待办事项中的 issue 描述了这项功能，但在编码前还需要就合并多个类别等细节达成一致。你将使用 Plan 模式确定这些决策，然后授权 Autopilot 在限定范围内实现功能。

## 背景

在开发流程中引入 AI 编码代理，并不会改变基本原则。事实上，这些原则会变得更加重要！大多数开发者遵循的流程大致如下：

1. 从已创建、描述待办工作的 issue 开始。
2. 制定构建方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这种标准方法，可以确保 AI 生成的代码符合要求，并经过与手动编写的代码相同的审核流程。

## 对话模式

**对话模式**决定代理拥有多大自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在不同模式间切换：

- **Interactive**：你与代理协作。代理提出变更建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作——编写代码、运行测试，并反复迭代，直到任务完成。

先从 Plan 模式开始，审查计划，然后使用 Autopilot 实现计划。

## 从 issue 开始

开始筛选功能的开发前，返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd>，打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd>，显示所有 issue。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中，然后返回 **Session** 选项卡。

请注意，此时提示会以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的 issue 或拉取请求（PR）加入上下文。

## 规划筛选功能

规划让你有机会在交由 Copilot 执行前，明确功能或任务的实施方案。遇到复杂任务时，花些时间规划总是值得的。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的 issue 引用后，输入以下提示：

   ```plaintext
   为实现此功能制定一个计划。
   ```

Copilot 会开始制定计划！它会先探索项目，再确定最佳方案。

3. 过程中，Copilot 可能会询问筛选功能应如何工作。根据你的偏好作答，没有错误答案！
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动以审查计划。
6. 要是计划的任何部分与你的决定不符，就让 Copilot 修改。

## 批准 Autopilot

计划编写并审查完毕后，就可以开始实现了！现在使用 Autopilot，让 Copilot 执行任务。

Autopilot 会让 Copilot 反复处理问题，直到它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或你所用版本中标签类似的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何按照已确定的计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后，实现可能会立即开始，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，和其他代码一样，合并前也需要审查。我们一起审查代码并运行网站，确保一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 将结果与 issue 以及规划时做出的决定进行比较。
4. 审查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的输出，并要求 Copilot 解决任何失败项。

## 体验新功能

好了，代码看起来没问题——但能运行吗？像之前一样启动应用，通过 Codespaces 转发的端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   启动应用，以便我在浏览器中试用筛选功能。告诉我 URL，并让服务器保持运行。
   ```

2. Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**。
3. 试用类别筛选、发行商筛选，以及计划中确定的组合方式。
4. 确认重置和无结果时的行为符合 issue 和你的决定。
5. 返回 Codespace，并要求 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已经使用不同的对话模式构建并审查了一项功能。本课中，你：

- 从 GitHub issue 开始了新的 Copilot 对话。
- 在 Plan 模式下明确了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，你将[使用自定义指令][next-lesson]深入了解如何生成符合文档规范的代码。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
