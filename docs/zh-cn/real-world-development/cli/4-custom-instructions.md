---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "了解存储库指令，添加文档规范，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，你需要让 Copilot 获得相应指导。[指令文件][instruction-files]不仅描述你想要什么代码，也说明代码应如何组织。现在你已经实现了筛选功能，接下来将了解 Copilot 使用的指令、添加文档规范，并将其应用于代码。

本课将带你：

- 了解存储库指令和按路径限定的指令文件如何传递给代理。
- 更新指令文件，确保遵循编码标准。
- 了解指令文件对代码的影响。

## 场景

优秀的开发团队都会为开发实践制定一套指南和要求，Tailspin Toys 也不例外。这些要求包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值（如果存在可注入的 `db` 参数，也要说明）。
- 可复用的 Astro 组件应说明其 `Props` 接口；相关代码变更时，注释也应保持更新。
- 保留现有格式设置和 lint 指南。

通过指令文件，你可以确保 Copilot 掌握正确信息，按照上述实践完成任务。

## 指令文件

自定义指令可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。它是一种强大的功能，可以让你指定首选编码规范、库，甚至是你喜欢添加的注释类型。你可以为整个存储库创建指令，也可以为特定文件类型或任务创建指令。

指令文件分为两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指南的文件。

> [!NOTE]
> 不同环境支持的其他指令格式可能有所不同。使用特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 查看项目中的自定义指令文件

入门项目已包含一组指令文件。先了解现有内容，再进行修改，以便观察其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，了解其中的项目简介和编码指南。这些指令适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了用于确定指令适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他指令文件和现有项目文件的链接。这样可以将较大的指令集拆分成较小、可复用的文件，并链接到示例。

## 更新指令文件以符合团队指南

现有文件已经打下良好基础，但仍有不足。我们将修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含文档注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，通常在第 35 行附近。
2. 在该部分末尾添加以下项目：

   ```markdown
   - 所有新编写的 TypeScript 都应包含用于文档说明的 TSDoc 注释。
   ```

文件会自动保存！

## 使用更新后的指南

Copilot CLI 会在对话开始时加载存储库指令。保存更新后的指令后，退出并恢复筛选会话，这样 Copilot 就能在保留功能上下文的同时加载新指南。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指南：

   ```plaintext
   我们刚刚更新了指令和代码指南。请按照这些指南更新你生成的代码。
   ```

4. 输入 `/diff`，阅读变更后的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何选取指令，并将新标准应用于功能。具体来说，你：

- 了解了存储库指令和按路径限定的指令文件如何传递给代理。
- 更新了指令文件，确保遵循编码标准。
- 了解了指令文件对代码的影响。

接下来，你将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件及其他资源汇总][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
