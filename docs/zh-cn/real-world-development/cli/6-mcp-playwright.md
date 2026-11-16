---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并使用它在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如前面所说，编写代码不只是写代码。我们还要处理数据、外部服务，并让 Copilot 能使用更多自动化工具。MCP 服务器正是为此而生。MCP 服务器能让 Copilot 超越 CLI 内置功能，使用更多工具和服务。

本课将带你：

- 了解 Model Context Protocol（MCP）是什么，以及 Copilot CLI 如何使用它。
- 如果尚未添加 Playwright MCP 服务器，则进行添加。
- 让代理操作浏览器并体验筛选功能。

## 场景

单元测试和端到端测试固然重要，但要验证 UI 更新，就必须实际与 UI 交互。你希望 Copilot 能像用户一样使用正在开发的网站，进一步自动化变更流程，并增强对更新按预期运行的信心。

## 什么是 Model Context Protocol（MCP）？

[Model Context Protocol（MCP）][mcp-blog-post]为 AI 代理提供了与外部工具和服务通信的方式。借助 MCP，AI 代理可以实时访问外部工具和服务，从而获取最新信息并代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 代理与外部工具和服务之间的桥梁。每个 MCP 服务器都提供一组 AI 代理可以访问的工具和资源。

目前有不少常用的 MCP 服务器，例如：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、issue 和拉取请求的 API 访问权限。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器可供使用。GitHub 提供了一个 [MCP 注册表][mcp-registry]，帮助大家发现生态系统中的项目并参与贡献。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前先检查源代码、验证发布者，并考虑相关安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 能像用户一样操作网站。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，直接继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 在名称字段中输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 将以下内容粘贴到 **Command** 对话框中：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux），保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 通过 Playwright 体验功能

之前你已手动确认功能按预期运行。现在使用 Copilot 中的 Playwright MCP 服务器再次检查！

1. 使用以下提示，让 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   启动应用，并使用 Playwright MCP 对照 issue 和计划检查筛选功能。不要进行任何修改，告诉我哪些功能正常、哪些不正常。完成后停止你启动的服务器。
   ```

> [!NOTE]
> 你不必明确告诉 Copilot 使用 MCP 服务器，因为它通常会自行判断。不过，既然你知道应该使用什么工具，明确指出也很有帮助！这样可以让结果更一致，还能节省一些 token。

2. 观察 Copilot 列出它在浏览器中执行的各项操作，以确认功能是否正常。
3. 阅读报告，确保所有行为都符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器，并向你提供报告。

## 总结和后续步骤

恭喜你通过 Copilot CLI 使用 Playwright MCP 在真实浏览器中体验了功能！回顾一下，你：

- 了解了 Model Context Protocol（MCP）是什么，以及 Copilot CLI 如何使用它。
- 如果 Playwright MCP 服务器尚未配置，则添加了它。
- 让代理操作浏览器并体验筛选功能。

接下来，你将[创建 QA 自定义代理][next-lesson]，将技能与浏览器工具结合起来，打造专业角色。

## 资源

- [MCP 究竟是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
