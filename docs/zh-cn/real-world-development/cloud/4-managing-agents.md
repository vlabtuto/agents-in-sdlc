---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几节练习中，你让 Copilot cloud agent 承担了三个独立任务，重点是改善用户体验和添加功能。Cloud agent 虽然可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个任务控制中心，你可以查看所有具有未关闭拉取请求（PR）的代理任务。你可以查看执行的操作，甚至可以引导正在进行的会话。

在本课中，你将：

- 浏览代理页面，监控 cloud agent 任务；
- 引导正在进行的会话，要求添加额外功能。

## 场景

将任务分配给代理以创建高对比度模式后，团队意识到现在也是添加浅色模式的好时机。既然正在更新网站样式并添加切换功能，顺便加入浅色模式就很合理。你希望引导代理的工作，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看分配给 Copilot cloud agent 的所有任务的当前状态。

1. 前往 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 留意主面板和左侧面板中的任务列表。你应该会看到分配给 Copilot 的任务，包括：
    - 更新代码文档；
    - 生成用于修改游戏的 API；
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作。这些操作可能包括：
    - 从存储库检出代码；
    - 为 Copilot 创建工作环境；
    - 设置 MCP 服务器；
    - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 列出的具体步骤会因 Copilot 的工作状态和采用的方法而异。

4. 同时留意右侧出现的拉取请求（PR）面板。你可以在其中查看 PR 和已更改的文件，以便进一步监控。

## 引导 cloud agent

现在你已经查看了正在运行的任务，可以要求 Copilot 在处理高对比度模式时添加浅色模式切换开关。

1. 选择表示正在添加高对比度模式的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 查看会话几分钟，直到会话显示已完成设置并开始工作。看到类似下方的消息时，就表示已经开始工作。
3. 在 **Copilot 工作期间引导活动会话**对话框中，输入以下提示：

    ```plaintext
    添加高对比度模式的同时，也为网站添加浅色模式。还应提供一个切换开关，让用户选择所需的显示模式。
    ```

    ![代理页面中的 cloud agent 任务截图，突出显示引导活动会话的对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续处理

和之前一样，Copilot 会开始处理更新后的任务！在你发送消息时，它会先完成当前步骤，然后将新请求纳入工作流程。

和前面一样，这需要几分钟，正好可以停下来回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 代理页面，它是监控和引导 GitHub Copilot cloud agent 任务的中心枢纽。借助这个任务控制中心，可以跟踪所有正在进行和已完成的任务、查看正在执行的工作，甚至可以重定向正在进行的任务以调整范围或提供更多指导。

本课介绍了以下概念：

- 使用任务控制中心和代理页面监控 cloud agent 任务；
- 重定向正在进行的会话，以要求添加额外功能。

Copilot 正在完成无障碍功能，现在我们可以转而[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
