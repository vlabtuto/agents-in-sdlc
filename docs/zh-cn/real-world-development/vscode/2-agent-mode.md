---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发人员，你通常需要找到所有相关文件、进行更改、运行测试、调试、找出遗漏的文件、再做一次更新……诸如此类。

这正是 Copilot 代理模式发挥作用的地方。

Copilot 代理模式可以在 IDE 中更自主地工作。它的行为类似开发人员：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。接下来，我们来看看如何使用代理模式为网站添加新功能。

> [!NOTE]
> 尽管名称相似，代理模式和云代理面向的是两种不同的体验。代理模式在 IDE 中执行任务，支持快速反馈和交互。云代理则像一位团队成员一样异步工作，通过问题和拉取请求与你协作。

在本练习中，你将了解：

- Copilot 代理模式如何探索项目、识别相关文件并协调更改；
- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能；
- 如何在将 Copilot 代理模式生成的更改合并到代码库之前，先审查更改和测试。

## 场景

随着游戏列表不断增加，你希望允许用户按类别和 publisher 进行筛选。上一练习已添加 publisher 辅助函数，现在将使用 Copilot 代理模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

开始更改之前，先探索 Tailspin Toys 网站，了解它目前有哪些功能。

该网站是一个带有开发人员主题的桌游众筹平台。它允许用户列出游戏并展示相关详情。该网站是一个 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过使用标准 DOM API 的精简 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，启动单个 Astro 开发服务器，让网站更容易运行。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，你应该已经切回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端窗口。
3. 创建并切换到用于此筛选工作的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，你会看到类似下面的横幅，其中显示服务器 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，从 Codespace 终端选择 localhost URL 会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通往 codespace 的私有隧道，此时 Web 服务器已在该 codespace 中运行！

### 浏览网站

网站运行后，可以探索它的功能。主要功能包括：

- **主页**：显示桌游列表，包括标题、图片和说明。
- **游戏详情页**：选择某个游戏后，会打开详情页，展示游戏的更多信息，包括标题、说明、publisher 和类别。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始一个干净的对话。每个练习对模式和模型的选择可能不同，相关步骤会在需要时说明。

网站的初始实现已经可以正常使用，但我们希望添加新功能。先查看待办事项。你从模板设置项目时，系统已自动创建一组问题作为待办事项，请 GitHub Copilot 显示这些内容。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。**Agent** 代理能够自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的代理选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊的作者并未偏好某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此将它列入操作说明，希望让你获得相对一致的代码建议和良好的体验。不过，由于 LLM 的输出具有概率性，你可能会发现得到的建议与工作坊中的说明不同。这完全正常，符合预期。

> [!NOTE]
> 由于 LLM 输出具有概率性，Copilot 可能会使用不同的 MCP 命令，但应该仍能完成任务。

3. 向 Copilot 发送以下提示，询问待办事项中的问题：

   ```plaintext
   请显示我的 GitHub 存储库中的待办事项，并根据对用户最有用的程度帮助我确定优先级。
   ```
4. 选择 **Continue** 以运行列出所有问题的命令。
5. 查看生成的问题列表。

留意 Copilot 还根据它认为对用户最有帮助的程度，为这些事项排了优先级。

## 查看说明文件

开始让代理生成代码之前，正好可以查看将为 Copilot 提供工作上下文的说明文件。你将使用[用户界面（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)说明文件，其中介绍了为网站添加功能时应采用的方法。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中的总体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍方面的原则；
   - 指向各类文件专属说明的链接，包括：
     - Astro；
     - Tailwind CSS；
     - Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目中的文件。路径相对于说明文件所在位置。这种方式便于复用、将较大的说明拆分成更易管理的小块，并提供示例和模板供 Copilot 参考。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行以下三项独立更新：

- 在数据层（`src/lib/`）添加或完善其余筛选逻辑；
- 添加或更新筛选行为测试；
- 更新游戏列表页面，加入筛选 UI。

此外，在将更改合并到代码库之前，还需要运行并通过测试。Copilot 代理模式可以帮你完成这些任务！现在就来添加此功能。

1. 可以继续当前与 Copilot 的对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉菜单中仍选中 **Agent**。

   ![Chat 视图中的代理选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办事项中的相关问题实现功能：

   ```plaintext
   请根据待办事项中相关 GitHub 问题的要求，更新网站以支持按 publisher 和类别筛选。上一练习已添加 publisher 辅助函数，请保留并根据需要完善这部分工作，同时完成其余的数据层、UI 和测试工作。完成前请确保所有测试均已通过。服务器已经在运行，无需再次启动。
   ```

6. 观察 Copilot 如何先探索项目并找到与目标功能相关的文件。你应该会看到它找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> 你会看到 Copilot 执行多项任务，例如探索项目、修改文件和运行测试。根据任务复杂度和代码库规模，可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会提供 **Keep** 或 **Undo** 选项，因此不必在工作过程中选择它们。

7. 按照 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中提示用户确认是否运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现暂停，甚至有些测试会失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务并且未检测到错误。

   ![Copilot 代理模式完整聊天会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请记住，务必审查 Copilot 或任何 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，探索新功能！
11. 确认一切正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和下一步

恭喜！本练习中，我们了解了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能。我们学习了：

- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能；
- Copilot 代理模式如何探索项目、识别相关文件并协调更改；
- 如何在将 Copilot 代理模式生成的更改合并到代码库之前，先审查更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]，然后为其创建拉取请求。

### 额外探索练习 – 实现分页

随着游戏列表不断增加，需要启用分页。运用本练习中学到的知识，提示 Copilot 更新网站以实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用现有说明文件；
- 考虑分页的实现方式，例如是否允许用户选择每页大小，还是将其设为固定值；
- 编写提示时，提供必要的指导，以便 Copilot 按照你的预期实现功能；
- 可能需要与 GitHub Copilot 多次交互，要求修改并补充上下文。这是使用 Copilot 时的正常流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
