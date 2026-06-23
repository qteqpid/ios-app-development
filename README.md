# iOS App Development Skills - SwiftUI iOS 应用开发 AI 技能库

语言：简体中文 | [English](README.en.md)

用于构建、审查和维护 SwiftUI iOS 应用的可复用 AI coding skills。这个仓库面向使用 Codex、Claude Code、AI coding agents 或自定义开发工作流的 iOS 开发者，提供 SwiftUI 应用架构、Swift 并发、SwiftData、本地文件导入、PDFKit/WebKit 阅读器、无障碍、性能优化、测试、App Store ASO 和发布说明等实践指南。

本仓库围绕 `my-ios-app-development` 这个顶层 iOS 应用开发最佳实践 skill 组织，并提供面向 SwiftUI 模式、并发、测试、性能、无障碍、SwiftData、PDFKit、WebKit、ASO 和 App Store 发布文案的专项 companion skills。

## 适合场景

- 用 Codex、Claude Code 或其他 AI coding assistants 开发、重构或审查 SwiftUI iOS 应用。
- 设计 iOS app architecture、状态管理、导航、主题、文件导入和本地优先数据流。
- 处理 Swift concurrency、`Sendable`、`@MainActor`、SwiftData、Swift Testing、PDFKit、WebKit 和 accessibility 相关实现。
- 优化 App Store metadata、ASO keywords、截图文案、TestFlight notes 和 App Store release notes。
- 建立可复用的 AI coding agent prompts、assistant skills 或 iOS development playbook。

## 搜索关键词

SwiftUI, iOS app development, AI coding skills, Codex skills, Claude Code skills, AI coding assistant, AI coding agent, iOS architecture, Swift concurrency, SwiftData, Swift Testing, PDFKit, WebKit, accessibility, iOS performance optimization, App Store ASO, App Store metadata, TestFlight notes, release notes, iOS development guide.

## Skills

| Skill | 用途 |
| --- | --- |
| `my-ios-app-development` | 顶层 iOS 应用开发最佳实践指南，覆盖架构、文件组织、状态所有权、持久化、主题、导入/分享流程、阅读器渲染表面、无障碍、性能、测试、ASO 元数据、发布说明和 Xcode 验证。 |
| `my-ios-app-swiftui-patterns` | SwiftUI 架构、状态所有权、导航、sheet、深度链接、preview、主题、控件、异步加载和视图组合模式。 |
| `my-ios-app-swift-concurrency` | Swift 并发诊断和修复，包括 `@MainActor`、`Sendable`、actor、任务取消和严格并发迁移。 |
| `my-ios-app-swift-testing` | Swift Testing 指南，覆盖 `@Test`、`#expect`、`#require`、异步测试、XCTest 迁移边界和测试组织。 |
| `my-ios-app-swiftui-performance` | SwiftUI 性能审查，覆盖失效刷新、身份变化、布局抖动、昂贵渲染工作、图片成本和 Instruments 工作流。 |
| `my-ios-app-ios-accessibility` | VoiceOver、Voice Control、Switch Control、Dynamic Type、键盘访问、自定义动作和 App Store 无障碍要求的实现与审查。 |
| `my-ios-app-swiftdata` | SwiftData 持久化细节，包括 `@Model`、`ModelContainer`、`@Query`、predicate、迁移、CloudKit 同步和 Core Data 共存。 |
| `my-ios-app-pdfkit` | PDFKit 渲染、导航、搜索、选择、缩略图、标注和 SwiftUI 包装。 |
| `my-ios-app-swiftui-webkit` | 基于 WebKit 的 SwiftUI HTML 渲染、本地内容、导航策略、JavaScript 和自定义 URL scheme。 |
| `my-ios-app-aso` | iOS App Store 元数据和 ASO 策略，包括应用名称、副标题、关键词、推广文本、描述、竞品矩阵、本地化 listing 文案、截图信息和定位审查。 |
| `my-ios-app-store-changelog` | 根据 git 历史生成 App Store “What's New”、TestFlight 说明和面向用户的变更日志。 |

## Layout

```text
skills/
  my-ios-app-development/
  my-ios-app-swiftui-patterns/
  my-ios-app-swift-concurrency/
  my-ios-app-swift-testing/
  my-ios-app-swiftui-performance/
  my-ios-app-ios-accessibility/
  my-ios-app-swiftdata/
  my-ios-app-pdfkit/
  my-ios-app-swiftui-webkit/
  my-ios-app-aso/
  my-ios-app-store-changelog/
```

每个 skill 都是自包含的，并带有必需的 `SKILL.md`。部分 skills 还包含 `references/`、`scripts/`、`evals/` 或 `agents/` 元数据。

## Local Installation

通过把 skill 文件夹链接到 Codex 的 skills 目录来安装：

```bash
ln -sfn "$PWD/skills/my-ios-app-development" ~/.codex/skills/my-ios-app-development
ln -sfn "$PWD/skills/my-ios-app-swiftui-patterns" ~/.codex/skills/my-ios-app-swiftui-patterns
ln -sfn "$PWD/skills/my-ios-app-swift-concurrency" ~/.codex/skills/my-ios-app-swift-concurrency
ln -sfn "$PWD/skills/my-ios-app-swift-testing" ~/.codex/skills/my-ios-app-swift-testing
ln -sfn "$PWD/skills/my-ios-app-swiftui-performance" ~/.codex/skills/my-ios-app-swiftui-performance
ln -sfn "$PWD/skills/my-ios-app-ios-accessibility" ~/.codex/skills/my-ios-app-ios-accessibility
ln -sfn "$PWD/skills/my-ios-app-swiftdata" ~/.codex/skills/my-ios-app-swiftdata
ln -sfn "$PWD/skills/my-ios-app-pdfkit" ~/.codex/skills/my-ios-app-pdfkit
ln -sfn "$PWD/skills/my-ios-app-swiftui-webkit" ~/.codex/skills/my-ios-app-swiftui-webkit
ln -sfn "$PWD/skills/my-ios-app-aso" ~/.codex/skills/my-ios-app-aso
ln -sfn "$PWD/skills/my-ios-app-store-changelog" ~/.codex/skills/my-ios-app-store-changelog
```

安装或重命名 skills 后，请重启 Codex 或打开一个新会话。

## Usage

将 `my-ios-app-development` 作为 iOS 应用工作的默认起点。需要更深入的具体框架 API 指南时，它会引导到专项 companion skills。

例如：

- 用 `my-ios-app-development` 处理功能规划、应用架构、数据所有权、文件布局、ASO 元数据和验证策略。
- 用 `my-ios-app-swiftui-patterns` 处理 SwiftUI 页面组合、导航、sheet、应用接线、preview 和状态所有权细节。
- 当编译器诊断涉及 actor isolation、`Sendable` 或数据竞争安全时，用 `my-ios-app-swift-concurrency`。
- 当需要生成或优化 App Store 名称、副标题、关键词、描述、截图文案或竞品矩阵时，用 `my-ios-app-aso`。
- 当需要根据 git 历史准备 App Store 或 TestFlight 发布说明时，用 `my-ios-app-store-changelog`。

## Attribution

部分 companion skills 或 references 改编自公开 skill 仓库。包含上游材料的 skill 目录中会保留对应的许可证说明。
