---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚才使用代理模式构建了筛选功能。创建拉取请求之前，应该在浏览器中确认功能确实正常。现在，无需亲自点击浏览应用，你将连接 **Playwright MCP 服务器**，让 Copilot 控制真实浏览器为你测试功能，然后发布分支并创建 PR。

在本练习中，你将：

- 了解什么是模型上下文协议（MCP），以及 MCP 服务器如何通过新工具扩展 Copilot 的能力；
- 将 Playwright MCP 服务器添加到工作区；
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能；
- 发布分支，并为筛选工作创建拉取请求。

## 什么是模型上下文协议（MCP）？

当代理模式能够访问编辑器以外的资源时，它的能力会大幅增强。Copilot 通过模型上下文协议（MCP）实现这一点：MCP 是一种让代理与外部工具和服务通信的标准方式。

![代理模式的内部工作原理及其与上下文、LLM 和工具的交互示意图，其中包括由 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)让 AI 代理能够与外部工具和服务通信。使用 MCP，AI 代理可以实时访问外部工具和服务，从而获取最新信息（通过资源）并代表你执行操作（通过工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 代理与外部工具和服务之间的桥梁，负责管理代理与外部工具之间的通信（例如现有 API 或 NPM 包等本地工具）。每个 MCP 服务器都提供一组 AI 代理可以访问的工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API。AI 代理可以使用它创建存储库、更新现有存储库，以及管理问题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力。AI 代理可以使用它访问网页、填写表单和选择按钮。

还有许多其他 MCP 服务器提供不同的工具和资源。GitHub 托管了一个 [MCP 注册表](https://github.com/mcp)，帮助用户发现并参与生态系统。

> [!CAUTION]
> 出于安全考虑，应像对待项目中的其他依赖项一样对待 MCP 服务器。使用之前，请仔细检查其源代码、验证发布者，并考虑相关安全影响。只使用你信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

此 `github` 条目由项目模板提供，因此上一练习中 Copilot 才能读取待办事项中的问题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容：VS Code 会通过 HTTP 连接该服务器，并在 Copilot 首次使用其工具时让你使用 GitHub 帐户进行身份验证。

## 添加 Playwright MCP 服务器

现在添加第二台服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好可以用来测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 条目，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志指示 Playwright 在后台运行浏览器，不显示窗口。codespace 中没有桌面环境，因此必须使用此选项。

> [!NOTE]
> Tailspin Toys 项目已将 Playwright 用于端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器提供的工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问你是否信任该服务器，请检查配置并选择信任，以便服务器启动。

手动启动并非必需。如果跳过此步骤，VS Code 会在下一步 Copilot 首次需要该服务器工具时启动服务器（并提示你信任它）。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]能让 Copilot 控制真实浏览器。无需亲自点击应用检查工作，代理可以打开页面、导航、应用筛选条件，并读取结果后向你汇报。它还能总结观察结果，因此无需离开当前对话就能快速确认功能是否符合预期。

在底层，Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]而不是截图。这意味着代理会分析带有标签的结构化元素（按钮、链接、列表项），与辅助技术的工作方式相同，因此快速功能检查也可以顺便进行简单的无障碍检查。

连接服务器并运行应用后，让 Copilot 测试刚刚构建的筛选功能：

```text
使用 Playwright MCP 服务器，在浏览器中打开正在运行的应用 http://localhost:4321，并验证新的游戏筛选功能：

1. 转到游戏页面，记下列出的游戏数量。
2. 应用类别筛选条件，确认列表只显示该类别的游戏。
3. 清除类别筛选，然后应用 publisher 筛选，确认列表更新为该 publisher 的游戏。
4. 同时应用类别和 publisher 筛选，确认结果同时符合两个条件。

报告每个步骤中的观察结果，并指出任何不符合预期的行为。
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐项执行上述步骤，并汇报发现。将它的总结与问题中的验收标准进行对照。如果发现异常，可以继续提问，或让它在创建拉取请求之前修复代码。

> [!NOTE]
> 要运行此测试，应用必须在 `http://localhost:4321` 上运行。如果已停止开发服务器，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），让团队进行审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板并检查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

创建拉取请求的方式有很多种，包括通过 github.com 和 GitHub 命令行界面（CLI）创建。不过，既然已经在使用 GitHub Copilot，不如让它代为创建 PR！它可以找到相关问题，并创建与该问题关联的 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认代理下拉菜单中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 让 Copilot 为你创建 PR：

   ```plaintext
   在存储库中查找与按类别和 publisher 筛选有关的问题。为当前分支创建新的拉取请求，并将其关联到正确的问题。
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会提示你通过 GitHub 身份验证；按照提示完成授权。
5. 留意 Copilot 如何搜索问题、找到正确的问题并创建 PR。
6. 选择 Copilot 生成的链接来查看拉取请求，但**暂时不要合并**。

## 总结和下一步

恭喜！在本练习中，你：

- 了解了模型上下文协议（MCP），以及 MCP 服务器如何通过新工具扩展 Copilot 的能力；
- 将 Playwright MCP 服务器添加到了工作区；
- 使用该服务器在浏览器中手动测试筛选功能，然后再发布；
- 发布了分支，并为筛选工作创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 能力的一种方式。接下来，来[创建自定义代理][next-lesson]，简化无障碍审查等专项任务。

## 资源

- [MCP 是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
