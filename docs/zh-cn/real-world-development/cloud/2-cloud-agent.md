---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会为技术债务所困扰。这些问题可能是尚未解决的安全问题、需要更新的旧代码，或是因为一直没有时间实现而积压的功能请求。GitHub Copilot 的 cloud agent 可以自主执行更新代码和添加功能等任务。代理完成工作后，会生成一份草稿 PR，供开发人员审核。这样既能将繁琐任务交给代理处理、加快开发流程，也能让开发人员专注于更重要的事项。

你将探索如何使用 Copilot cloud agent：

- 自定义生成代码的环境；
- 确保安全地执行操作；
- 了解明确界定 issue 范围的重要性；
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站初版的承包商没有留下完善的文档——你会发现，实际上完全没有文档。首先，他们希望为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望每个游戏的详情页都显示相关游戏，也就是同一类别中的其他游戏。暂时不需要完善的实现，只需要一个可用于 UX 验收测试的版本。目前这项工作仍是一个阻塞项，不过还有优先级更高的 issue。

这两项任务都可能很快被降低优先级，因此很适合交给 Copilot cloud agent。它可以异步处理任务，让开发人员先专注于其他工作，之后再回来审核 Copilot 的工作并确认结果符合预期。

## 认识 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发人员一样在后台执行任务。就像与人类开发人员协作一样，你可以通过多种方式分配工作，包括[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。在任务进行期间或完成后，你都可以查看 Copilot 会话。提出的解决方案准备好供你审核时，Copilot 会在拉取请求中标记你。

## 明确界定说明范围的重要性

GitHub Copilot 并非魔法——它不能替你思考。即使看似简单的操作，深入分析后也会发现其复杂性，因此[为 Copilot cloud agent 界定任务范围时要谨慎][cloud-agent-best-practices]。请将它视为 AI 结对编程助手：分阶段开展工作、学习、试验并不断调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义说明

前面的练习介绍了自定义说明及其如何指导 Copilot。如果你已经完成自定义说明的实践练习，就已经见识过 `.github/copilot-instructions.md` 的作用；如果还没有，现在正好可以快速浏览一下。将工作分配给 Copilot cloud agent 之前，先只读查看此存储库中已有的说明文件，之后就能留意它们的影响。

在存储库的 GitHub 网站 UI 中打开以下文件；如果已有 codespace，也可以在其中打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其留意 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将这些说明限定为适用于 `**/*.test.ts` 文件。

在下一节将 `Code lacks documentation` issue 分配给 cloud agent 后，留意生成的拉取请求是否采用了 TSDoc 文档注释、TypeScript 约定和注释头。这些要求来自上述说明文件。稍后练习中审核 PR 时还会再次提到这一点。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保环境处于良好状态。将任务分配给 Copilot 时也是如此，因为它会以类似软件工程师的方式执行任务。

Cloud agent 使用 [GitHub Actions][github-actions] 执行工作。你可以创建一个[专用设置工作流][setup-workflow]来自定义环境，该工作流配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，并在 agent 开始工作前运行，以便它能使用所需的开发工具和依赖项。此工作流已提前配置好，以便本实验流程顺利进行并提供学习机会。它会确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单个 Astro 应用已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设置为*你的步骤*所需的最低权限。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 如果希望在设置步骤中克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端 / 应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

它看起来和其他 GitHub 工作流文件类似，但有几个要点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，会在 GitHub Actions 中运行此作业。
- 留意 `workflow_dispatch` 触发器。它允许你从存储库的**操作**选项卡手动运行工作流。这很适合用于测试工作流是否成功运行，而不必等待 Copilot 启动它。

## 添加文档

每个人都知道文档很重要，但大多数项目的信息要么过时，要么完全缺失。这类技术债务往往得不到处理，进而降低工作效率，也增加维护代码库或引入新开发人员的难度。幸运的是，Copilot 很擅长创建文档，因此这是分配给 Copilot cloud agent 的理想任务。它会在后台生成所需文档。之后的练习中，你会回来审核其工作。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择**议题**选项卡。
3. 选择**新建议题**，打开新建议题对话框。
4. 选择**空白议题**，创建空白议题。
5. 将**标题**设置为 `代码缺少文档`。
6. 将**说明**设置为：

    ```plaintext
    我们组织要求在有助于理解行为的函数和方法中添加 TSDoc 文档注释。遗憾的是，近期更新没有遵循此标准。我们需要更新现有代码，在文档注释能阐明行为时添加注释。
    ```

7. 选择**创建**，创建议题。
8. 在右侧选择**分配给 Copilot**，打开分配对话框。

  ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择**分配**。

10. 选择**拉取请求**选项卡。
11. 打开新生成的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应该会创建一个待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定处理问题的最佳方案。

13. 查看列表及其计划完成的任务。
14. 向下滚动拉取请求时间线，你应该会看到 Copilot 已开始处理 issue。
15. 选择**查看会话**按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 留意实时会话中的内容，以及 Copilot 如何解决问题。这包括探索代码并了解当前状态、暂停思考并决定合适的计划，以及编写代码。

这可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让我们能专注于其他工作。接下来我们会利用这一点：先将另一项任务分配给 Copilot cloud agent，然后编写代码为应用程序添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot cloud agent 的一大优势是可以拆分工作：你专注于一组任务，而它处理另一组任务。为设计团队添加相关游戏部分可能不会花太长时间，但这段时间仍可用于处理其他任务。现在就把工作分配给 Copilot cloud agent！

1. 返回 github.com 上的存储库。
2. 选择**议题**选项卡。
3. 选择**新建议题**，打开新建议题对话框。
4. 选择**空白议题**，使用空白模板。
5. 将**标题**设置为 `在游戏详情页显示相关游戏`。
6. 将**说明**设置为：

    ```markdown
    我们希望在每个游戏的详情页显示相关游戏，帮助用户发现更多游戏。设计团队希望探索 UX 并进行验收测试。具体要求如下：

   - 在 `src/lib/` 中添加一个数据访问辅助函数，返回给定游戏所在类别中的其他游戏，并排除该游戏本身
   - 在游戏详情页显示“相关游戏”部分，并使用此辅助函数
   - 处理游戏没有相关游戏的情况
   - 为新辅助函数创建单元测试
   - 创建 PR 之前，确保所有测试均通过
   ```

7. 选择**创建**，创建议题。
8. 在右侧选择**分配给 Copilot**，打开分配对话框。

  ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择**分配**。

不久之后，你应该会在 issue 的第一条评论中看到一组 👀，表示 Copilot 已开始处理！

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理你的第二项请求！Copilot cloud agent 的工作方式类似软件工程师，因此你不需要主动监控它，而是可以等任务完成后再审核。现在，我们把注意力转向创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。使用 cloud agent，可以将 issue 分配给 Copilot 并让它异步处理。你可以使用 Copilot 处理技术债务、创建新功能，或协助将代码从一种框架迁移到另一种框架。

本课介绍了以下概念：

- 自定义生成代码的环境；
- 确保安全地执行操作；
- 明确界定 issue 范围的重要性；
- 将 issue 分配给 Copilot。

Copilot cloud agent 正在后台处理任务，现在我们可以转而创建和使用自定义代理。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并且可以访问自定义说明；我们已在前面的模块中介绍过自定义说明。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
