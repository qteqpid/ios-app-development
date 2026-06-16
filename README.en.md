# iOS App Development Skills

Language: [简体中文](README.md) | English

Reusable Codex skills for building, reviewing, and maintaining SwiftUI iOS apps.

This repository is organized around `my-ios-app-development`, a top-level best-practice skill for iOS app work, plus focused companion skills for SwiftUI patterns, concurrency, testing, performance, accessibility, SwiftData, PDFKit, WebKit, ASO, and App Store release copy.

## Skills

| Skill | Purpose |
| --- | --- |
| `my-ios-app-development` | Top-level iOS app development best-practice guide covering architecture, file organization, state ownership, persistence, theming, import/share flows, reader rendering surfaces, accessibility, performance, testing, ASO metadata, release notes, and Xcode validation. |
| `my-ios-app-swiftui-patterns` | SwiftUI architecture, state ownership, navigation, sheets, deep links, previews, theming, controls, async loading, and view composition patterns. |
| `my-ios-app-swift-concurrency` | Swift concurrency diagnostics and remediation, including `@MainActor`, `Sendable`, actors, task cancellation, and strict concurrency migration. |
| `my-ios-app-swift-testing` | Swift Testing guidance for `@Test`, `#expect`, `#require`, async tests, XCTest migration boundaries, and test organization. |
| `my-ios-app-swiftui-performance` | SwiftUI performance review for invalidation, identity churn, layout thrash, expensive render work, image cost, and Instruments workflows. |
| `my-ios-app-ios-accessibility` | Accessibility implementation and audits for VoiceOver, Voice Control, Switch Control, Dynamic Type, keyboard access, custom actions, and App Store accessibility expectations. |
| `my-ios-app-swiftdata` | SwiftData persistence details, including `@Model`, `ModelContainer`, `@Query`, predicates, migrations, CloudKit sync, and Core Data coexistence. |
| `my-ios-app-pdfkit` | PDFKit rendering, navigation, search, selection, thumbnails, annotations, and SwiftUI wrapping. |
| `my-ios-app-swiftui-webkit` | WebKit-backed SwiftUI HTML rendering, local content, navigation policy, JavaScript, and custom URL schemes. |
| `my-ios-app-aso` | iOS App Store metadata and ASO strategy for app names, subtitles, keyword fields, promotional text, descriptions, competitor matrices, localized listing copy, screenshot messaging, and positioning review. |
| `my-ios-app-store-changelog` | App Store "What's New", TestFlight notes, and user-facing changelog generation from git history. |

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

Each skill is self-contained and has a required `SKILL.md`. Some skills also include `references/`, `scripts/`, `evals/`, or `agents/` metadata.

## Local Installation

Install by linking the skill folders into Codex's skills directory:

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

Restart Codex or open a new session after installing or renaming skills.

## Usage

Use `my-ios-app-development` as the default starting point for iOS app work. It routes framework-specific questions to the focused companion skills when deeper API guidance is needed.

For example:

- Use `my-ios-app-development` for feature planning, app architecture, data ownership, file layout, ASO metadata, and validation strategy.
- Use `my-ios-app-swiftui-patterns` for SwiftUI screen composition, navigation, sheets, app wiring, previews, and state ownership details.
- Use `my-ios-app-swift-concurrency` when compiler diagnostics involve actor isolation, `Sendable`, or data-race safety.
- Use `my-ios-app-aso` when generating or optimizing App Store names, subtitles, keywords, descriptions, screenshot copy, or competitor matrices.
- Use `my-ios-app-store-changelog` when preparing App Store or TestFlight release notes from git history.

## Attribution

Some companion skills or references were adapted from public skill repositories. When a skill includes upstream material, its directory contains the relevant license notice.
