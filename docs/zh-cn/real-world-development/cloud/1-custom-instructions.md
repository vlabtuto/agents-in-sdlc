---
title: "练习 1 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果某项任务需要以特定方式完成，或者有 Copilot 应该了解的背景信息，就需要确保它能访问这些上下文。[说明文件][instruction-files]可以提供这些指导，让 Copilot 不仅了解要做什么，也了解应该如何完成。

在本练习中，你将：

- 了解如何通过存储库自定义说明和按路径指定的说明文件，将项目专属上下文、编码指南和文档标准传达给 Copilot；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 实践环境不同，这里不会运行“前后对比”提示，因为 Copilot cloud agent 会异步处理 GitHub issue，因此很难实时并排展示影响。稍后查看 cloud agent 在本课程实践环节中生成的拉取请求时，你会看到说明文件发挥的作用。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求。这些要求包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式，并具有现代感。
- 应以 TSDoc 文档注释的形式在代码中添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用说明文件，可以确保 Copilot 获得正确的信息，并按照上述实践完成任务。

### 自定义说明

自定义说明可以为 Copilot 提供上下文和偏好设置，让它更好地理解你的编码风格和要求。借助这一强大功能，你可以指定偏好的编码约定、库，甚至希望添加的注释类型。你可以为整个存储库创建说明，也可以针对特定类型的文件创建说明，为具体任务提供上下文。

说明文件有两种类型：

- `.github/copilot-instructions.md`：每次针对该存储库发送请求时，都会将这个唯一的说明文件提供给 Copilot。此文件应包含项目级信息，即与发送给 Copilot 的大多数聊天或 CLI 请求相关的上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建这些文件。例如，可以为 TypeScript 或 Astro 等特定语言编写说明，也可以为创建 UI 组件或一组单元测试等任务编写说明。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不适用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时，都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理说明文件的最佳实践

关于如何创建说明文件的讨论超出了本工作坊的范围。不过，示例项目展示了具有代表性的做法。总体而言：

- 将 `copilot-instructions.md` 中的说明集中在项目级指导上，例如项目的构建内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言。确保指导清晰，并提供代码示例，说明代码应该和不应该采用什么形式。

创建说明文件没有唯一的方法，使用 AI 也是如此。通过实验，你会找到适合自己项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的说明文件。浏览此项目中的文件时，你可能会注意到其中包含多种任务的说明，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 还可以帮助你生成说明文件。不同产品界面的操作方式各不相同（例如，VS Code 中的**配置聊天 → 生成代理说明**，或 Copilot CLI 中的 `/init`）；你当前使用的产品界面的课程会在相关处说明。
>
> 想查找模板或入门示例？可以浏览 [awesome-copilot][awesome-copilot]，其中收集了说明文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览此项目中的自定义说明文件

花点时间阅读此存储库提供的说明文件：其中有一个核心文件 `copilot-instructions.md`，还有一组适用于各种任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub 网站 UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目的简要说明以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。嵌套在 **Code standards** 下的 **GitHub Actions Workflows** 指导也适用于你与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置了一个相对于存储库根目录的 glob，用于确定说明适用的文件。在此例中，所有匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些说明。
5. 留意此项目中有关创建单元测试的具体说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成更小、可重复使用的文件，并为 Copilot 提供可参考的示例。这些路径相对于说明文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但尚未要求在代码中添加文档。接下来的步骤会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来要编辑的内容只有一项：添加一条项目范围的规则，要求以 TSDoc 文档注释和文件级注释头的形式在代码中编写文档。稍后在本课程实践环节中为 cloud agent 分配 issue 时，它会应用此规则。

编辑之前，请创建一个分支以便开展工作（cloud agent 会从 issue 目标分支读取说明，但仍然要将本次编辑整洁地提交）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在将编码标准写入存储库，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应位于第 27 行附近。留意这里记录的项目编码标准，但其中尚未包含代码文档规则，这正是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - 每个导出的函数都应有 TSDoc 注释，说明其用途、参数和返回值。
   - 在导入语句或任何代码之前，为文件添加注释块，说明文件的用途。
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建说明文件，为整个项目提供指导；也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供说明。存储库级文件适合放置项目级标准，例如刚刚添加的文档注释规则。

## 提交、推送并合并说明更新

Copilot cloud agent 会读取 issue 目标分支中的说明文件。下一练习中，将 issue 分配给 Copilot 时，它会从 `main` 分支创建分支，因此必须将说明更改合并到 `main`，cloud agent 才能使用它们。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。这样，cloud agent 在下一练习中处理分配给它的 issue 时就会读取这些说明。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过分支步骤，直接提交到 `main`。此处创建分支是为了让工作坊流程贴近真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何从此项目的说明文件中获取上下文，并在 `.github/copilot-instructions.md` 中添加了一条新的存储库级标准。接下来几节中，Copilot cloud agent 生成的拉取请求会体现这条标准。

下一步，你将[把第一个 issue 分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件及其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
