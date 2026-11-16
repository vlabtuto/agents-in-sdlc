---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果某项任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能够获取这些上下文。向 Copilot 提供特定上下文的方法有很多，其中最重要的是[说明文件][instruction-files]，它们可以为代码生成提供指导。

在本练习中，你将：

- 了解项目专属上下文、编码指南和文档标准如何通过存储库自定义说明及路径范围说明文件传递给 Copilot；
- 在当前说明生效的情况下，生成用于筛选的第一部分数据层代码（publisher 辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准；
- 重新运行相同提示，观察生成的代码如何采用新标准；
- 将说明更新和筛选代码提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能与部分设定标准有所不同。Copilot 的输出具有非确定性。本练习旨在观察更新说明后行为的*整体趋势*，而不是要求输出逐字一致。

## 说明文件

### 场景

作为一家优秀的软件开发公司，Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 数据层必须始终配备单元测试。
- UI 应采用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明该文件的用途。

通过使用说明文件，可以确保 Copilot 获得正确的信息，并按照上述实践完成任务。

### 自定义说明

自定义说明可以向 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。它是一种强大的功能，可以帮助你获得更相关的建议和代码片段。你可以指定偏好的编码约定、库，甚至喜欢使用的注释类型。你可以为整个存储库创建说明，也可以为特定类型的文件创建说明，为具体任务提供上下文。

说明文件有两种类型：

- `.github/copilot-instructions.md` 是一个说明文件，会随该存储库的**每个**请求一起发送给 Copilot。文件应包含项目级信息，即 Copilot 收到的大多数聊天或 CLI 请求都适用的上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 文件可针对特定任务或文件类型提供指导。你可以为特定语言（如 TypeScript）或任务（如创建 UI 组件或一组单元测试）提供指南。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云代理在生成代码时，都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本工作坊的范围。不过，示例项目展示了一种有代表性的方法。总体而言：

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（如单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，确保指导清晰，并提供代码示例说明应采用和避免的写法。

创建说明文件没有唯一的方法，使用 AI 也是如此。通过实践，你会找到适合自己项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一组完善的说明文件。浏览此项目中的文件时，你可能会发现有许多任务类型的说明，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助你生成说明文件。不同界面的操作方式各不相同（例如，VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）。当前界面的课程会在相关处说明。
>
> 需要模板或起点吗？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量说明文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览此项目中的自定义说明文件

花一点时间阅读此存储库提供的说明文件：其中有一个核心文件 `copilot-instructions.md`，以及一组适用于各种任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目的简要说明以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。这里有 Astro 文件、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它会设置一个 glob（相对于存储库根目录），用于确定说明适用的文件。这里，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会匹配。
5. 查看此项目专门用于创建单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意指向其他说明文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成更小、可复用的文件，并向 Copilot 提供可参考的示例。（其中的路径相对于说明文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但目前尚未要求提供代码内文档。接下来的步骤会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为更改创建一个分支。

1. 从上一练习返回 codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始一个干净的对话。每个练习对模式和模型的选择可能不同，相关步骤会在需要时说明。

要了解自定义说明的影响，先在当前说明文件生效时发送提示。稍后更新说明后，再发送相同提示以观察差异。

1. 关闭之前练习中打开的编辑器标签页，以便 Copilot 仅获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应该放在哪里。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，以便 Copilot 应用文件更改。

   ![Chat 视图中的代理选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，为每个 publisher 返回其名称和 ID。应用文件更改。
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件及其测试。
6. 留意建议的辅助函数是一个类型化函数：它将 `db` 客户端作为第一个参数，并返回类型化的 publisher 数组。这些内容来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定（该文件适用于 `src/lib/*.ts`）。
7. 留意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有概率性，因此即使没有明确要求，也可能添加文档注释。如果发生这种情况也没关系，本练习的重点仍是观察更新说明后行为的一致性如何改善。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。我们来记录存储库的编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应位于第 27 行附近。留意其中记录了项目的编码标准，但尚未规定代码内文档要求，这就是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - 每个导出的函数都应有 TSDoc 注释，说明其用途、参数和返回值。
   - 在导入语句或任何代码之前，添加一个注释块说明文件用途。
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建适用于全局的说明文件，也可以针对特定语言、文件类型或任务创建 `*.instructions.md` 文件。存储库级文件适合放置项目级标准，例如刚添加的文档注释规则。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清除对话内容。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送之前的**相同提示**：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，为每个 publisher 返回其名称和 ID。应用文件更改。
   ```

4. 留意现在建议的文件开头包含类似以下内容的注释块：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的 Publisher 数据访问辅助函数。
    * 提供用于从数据库检索 publisher 信息的函数。
    */
   ```

5. 留意现在建议的函数包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有 publisher 的 ID 和名称。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为 publisher 对象数组。
    */
   ```

现在，你已引导 Copilot 遵循新的项目标准，并将其应用到下一练习会继续使用的实际代码中。

## 提交说明并推送分支

说明文件与存储库中的其他资源一样，使用相同的源代码管理方式。现在提交并推送分支到存储库。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端窗口。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新，以及 Copilot 为辅助函数基础所做的所有文件更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他配套更新（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，需要创建拉取请求，并将其关联到待办事项中的文档标准问题。我们可以手动操作，不过也可以让 Copilot 帮忙：提示它查找问题、创建用于解决该问题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上操作。项目模板已将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用其中某个工具时，VS Code 可能会提示你登录 GitHub。按照提示完成登录即可。下一练习会介绍这种连接的工作原理（模型上下文协议）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在代理选择器中选中 **Agent**。
3. 让 Copilot 查找与更新说明文件有关的问题，并从当前分支创建 PR，说明其中包含说明更新和新的 publisher 辅助函数基础：

   ```
   查找与更新说明文件有关的问题。从当前分支创建新的 PR，注明该 PR 会关闭此问题，并说明我们还为后续的筛选工作添加了 publisher 辅助函数基础。
   ```

4. Copilot 将开始查找问题并创建 PR。
5. 如果 Copilot 提示你**允许**它代表你执行 GitHub 操作，请查看命令，并酌情选择 **Allow**。
6. 创建 PR 后，让 Copilot 合并 PR，并使用以下提示将当前分支切回 main：

   ```
   将 PR 合并到 main。然后在本地切回 main，并拉取最新代码以确保代码已更新。
   ```

7. 如果 Copilot 提示你允许它代表你执行 GitHub 操作和运行 shell 命令，请查看命令，并酌情选择 **Allow**。

现在，你已经在 GitHub Copilot 的帮助下创建并合并了一个拉取请求！

## 总结和下一步

你了解了 Copilot 如何从此项目的说明文件获取上下文，并在 VS Code 中使用 Copilot Chat 完成了以下操作：

- 发送代码生成提示，观察 Copilot 根据*现有*说明生成的内容；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准；
- 重新运行相同提示，观察建议的代码如何采用新标准；
- 将说明和辅助函数基础提交到 `main`，为下一练习做好准备。

接下来，在[代理模式][next-lesson]中运用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件和其他资源汇总][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
