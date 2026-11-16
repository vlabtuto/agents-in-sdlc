---
title: "第 2 课 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中检查变更，并将其作为你的第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以首次修改项目了。先从小处着手：游戏数据中已经有星级评分，但主页上的游戏卡片还没有显示它。你将让代理显示评分、审查变更，并将其作为你的第一个拉取请求合并。

本课将带你：

- 在功能分支上开始一次专注于特定任务的 Copilot 对话。
- 让代理对项目进行小幅修改。
- 使用 `/diff` 审查变更。
- 运行应用，并在转发的浏览器中确认变更。
- 创建并合并你的第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，而且游戏详情页已经显示了评分。不过，主页上的游戏卡片只显示标题、类别、发行商和描述。作为热身练习，你将让代理在每张卡片上显示现有评分。这项小而独立的修改很适合作为你的首次会话练习。

## 对话的组成

**对话**是你使用 Copilot CLI 执行任务的地方。与 Copilot 应用不同，常规 CLI 对话使用当前终端中已检出的存储库和 Git 分支，而不是创建专用工作树。保存的对话可以让你稍后回到同一讨论，而文件和分支仍是磁盘上普通的 Git 状态。

对话中有三项内容：你的提示和代理的回复、代理探索和编辑文件时使用的工具，以及可通过 `/diff` 检查的变更。

## 开始对话并提出修改请求

我们先开始一个新对话，实现这项功能。

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未启动，请运行以下命令：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保开始一个新会话。
5. 使用以下提示请求修改：

   ```plaintext
   在列表页的游戏卡片上显示每款游戏的星级评分（starRating，满分 5 分）。如果评分为 null，则显示“暂无评分”。保持卡片布局不变，添加测试，并运行相关检查。
   ```

Copilot 会探索项目、找到用于显示游戏详情的文件，并创建所需代码。你已经使用 Copilot CLI 添加了一项新功能！

## 审查差异

所有 AI 生成的变更都应该在合并前审查，即使是小改动也不例外。现在就在 Copilot CLI 中检查这些变更。

1. 输入 `/diff` 并检查每个变更文件。
2. 确认游戏卡片在有评分时显示数字评分，在 `starRating` 为 `null` 时显示“暂无评分”。
3. 确认测试覆盖这两种情况。
4. 检查 Copilot 运行的检查结果，并要求它修复任何失败项。
5. 完成审查后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的输出具有概率性而非确定性，因此你的代码可能与示例不同。请检查行为，不要期待某一种固定实现。

## 检查变更

当然，我们不能只读代码就假定它能正常工作。让 Copilot 启动网站，然后在 Codespaces 转发的浏览器中检查更新后的用户界面（UI）。

1. 让 Copilot 启动应用：

   ```plaintext
   启动应用，以便我在浏览器中检查星级评分变更。告诉我 URL，并让服务器保持运行。
   ```

2. Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了满分为五分的评分。
4. 返回 Copilot，并要求它停止刚才启动的服务器：

   ```plaintext
   停止你刚才启动的开发服务器。
   ```

## 创建并合并你的第一个拉取请求

你已经完成了这项功能！现在创建一个拉取请求（PR），将新代码合并到项目中。

1. 让默认代理提交变更：

   ```plaintext
   使用合适的提交消息，提交已审查的星级评分变更。
   ```

2. 输入 `/pr create`。Copilot CLI 创建 PR 时会推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），同时选择 Copilot CLI 显示的 URL，以打开 PR。
4. 检查变更文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，并输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main` 分支：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜你使用 GitHub Copilot CLI 完成了第一个变更！具体来说，你：

- 在功能分支上开始了一次专注于特定任务的 Copilot 对话。
- 引导代理修改游戏卡片。
- 使用 `/diff` 审查变更。
- 运行应用，并在转发的浏览器中确认星级评分。
- 创建并合并了第一个拉取请求。

接下来，你将[从筛选 issue 入手，使用 Plan 和 Autopilot 模式][next-lesson]来构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
