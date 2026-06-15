---
name: my-ios-app-development
description: Use as the top-level best-practice guide when designing, implementing, refactoring, or reviewing SwiftUI iOS apps. Covers app architecture, file organization, state ownership, local-first data, theming, import/share flows, reader rendering surfaces, accessibility, performance, testing, concurrency safety, settings, App Store metadata, ASO competitor analysis, release notes, and Xcode build validation. For deep framework details, pair with specialized skills such as my-ios-app-swiftui-patterns, my-ios-app-swift-concurrency, my-ios-app-swift-testing, my-ios-app-swiftui-performance, my-ios-app-ios-accessibility, my-ios-app-swiftdata, my-ios-app-pdfkit, my-ios-app-swiftui-webkit, or my-ios-app-store-changelog.
---

# iOS App Development

Use this as the app-development best-practice skill for SwiftUI iOS work. It is especially useful for product design, feature implementation, UI polish, data modeling, local persistence, import/export flows, settings pages, project cleanup, and code review.

This skill sets the default engineering posture. When the task needs concrete framework APIs, use the relevant companion skill for details rather than expanding this guide with long examples.

## Development Principles

- Build the actual usable app experience first, not a marketing shell.
- Keep changes tightly scoped to the requested behavior.
- Prefer the app's existing patterns, naming, file layout, and helper APIs.
- Follow single-responsibility and easy-extension principles. Each file should usually own one focused concern.
- Reuse shared helper logic instead of duplicating parsing, formatting, persistence, or UI state code.
- Avoid introducing large frameworks, databases, or architecture changes unless the current approach is clearly limiting the feature.
- Make user-owned data durable before optimizing derived caches or visual polish.
- Preserve existing data formats and migration paths unless the task explicitly calls for a storage change.
- Treat build success as part of the task, not a separate optional step.

## Optimization Discussion Template

When the user wants to review optimizations or bug fixes before implementation, explain one item at a time and wait for confirmation before editing nontrivial code.

Use this shape:

1. Name the optimization and point to the relevant file/line when practical.
2. Explain the current behavior in concrete terms.
3. Explain the bug, risk, or maintenance cost it creates.
4. Recommend a specific implementation plan.
5. Call out tradeoffs, behavior changes, or compatibility questions.
6. State the exact next action you will take if the user confirms.

## Architecture And State

Default to a lightweight Model-View style for SwiftUI apps unless the project already uses another architecture.

- Views should be lightweight state expressions. Models, stores, importers, parsers, renderers, and services own behavior.
- Do not add view models by habit. Add a separate controller/store only when it owns real state, side effects, persistence, parsing, or coordination.
- Prefer `@State` for view-owned values and observable objects, `let` for read-only injected observable models, `@Bindable` when a child needs two-way bindings, and `@Environment` for app-wide shared services or stores.
- Mark UI-bound observable stores/classes as `@MainActor`.
- Keep navigation, sheets, selection, and transient UI state near the screen that owns them.
- Inject services through initializers or environment instead of reaching into global state from feature views.
- Use explicit enums for loading, error, render mode, sort order, import state, and reader mode instead of loose strings or parallel booleans.

Use `my-ios-app-swiftui-patterns` for detailed `@Observable`, environment, composition, async loading, and state-ownership guidance.

## File Organization

Use an MVC-style separation unless the existing project clearly uses another convention:

- **Model files** define base data objects and value types, such as `Book`, `User`, `ReadingHistoryEntry`, or app settings structs.
- **View files** define SwiftUI UI only. Keep them declarative and avoid file, parser, or persistence details inside views.
- **Control/helper files** contain observable stores, importers, parsers, formatters, cache managers, and other controller-style logic.

Practical rules:

- Keep `ContentView.swift` small. Use it mainly for top-level composition, navigation, sheets, and environment wiring.
- Split views by feature when a file grows: headers, grids, tiles, settings rows, reader controls, dialogs, and sheets can live in separate files.
- When the source folder around `ContentView.swift` becomes crowded, introduce responsibility-based subdirectories instead of leaving every feature file in one flat directory. Prefer domain/ownership groupings such as `Reader/Reflowable`, `Reader/Selection`, `Reader/Web`, `Library/Grid`, or `Library/Sheets` over arbitrary type buckets when that makes navigation clearer.
- Keep reusable non-UI behavior in focused helper files, for example `LocalFileImporter`, `BookParser`, `CacheManager`, or `ShareExporter`.
- Add a brief file-level comment only when the file's role, ownership boundary, lifecycle assumptions, or interaction with persistence/rendering/import flows is not obvious from its name and top-level types.
- Keep UI independent from storage paths. Views should render model values and call store/controller methods.
- Avoid mixing app-wide theme tokens directly into feature logic.

## Data And Persistence

- Start with the minimum model fields that support current behavior.
- Store stable identifiers separately from display names. Display names can change; IDs should not.
- Use explicit enums for states, formats, modes, and sort orders instead of loose strings.
- Persist user-owned data under `Application Support` unless the file should be user-visible in `Documents`.
- Keep generated caches, thumbnails, temporary share files, and derived render data separate from canonical user data.
- Prefer JSON persistence for small MVP datasets. Move to SwiftData/CoreData only when querying, migration, relationships, or scale justify it.
- For imported files, store metadata separately from the original managed file so the UI can load quickly.
- For large imported files, store the managed file and lightweight metadata separately. Do not load full document data just to render lists.
- Compute content hashes when duplicate detection matters.
- Treat migrations as product behavior: preserve existing libraries, reading progress, bookmarks, and settings.
- If adopting SwiftData, define schema ownership, migration strategy, preview/test containers, and CloudKit implications before changing persistence.

Use `my-ios-app-swiftdata` when implementing or reviewing `@Model`, `ModelContainer`, `@Query`, migrations, CloudKit sync, or Core Data coexistence.

## Theming

- Define theme modes and component color tokens in a dedicated theme file.
- Keep raw colors out of feature views.
- Add semantic tokens such as `background`, `primaryText`, `secondaryText`, `controlBackground`, `divider`, and `accent`.
- If a screen has a distinct visual language, give it a focused theme type instead of overloading the main app theme.
- Check both light and dark modes for contrast, readability, and white-on-white or black-on-black control failures.

## SwiftUI UI Guidance

- Prefer SwiftUI-native components such as `Menu`, `Sheet`, `NavigationStack`, `ShareLink`, or `UIActivityViewController` wrappers before building custom controls.
- Use small view structs with clear inputs and callbacks.
- Keep gestures predictable. If tap, long press, drag, and pinch coexist, isolate gesture state and test conflict cases.
- For dynamic grids, compute spacing from the container width and keep fixed item counts stable when the design requires it.
- Use `zIndex`/`ZStack` for overlay controls that should occupy the same visual layer, and hide mutually exclusive overlays instead of stacking them vertically.
- Add animation where layout changes would otherwise feel abrupt, such as deletion, menu appearance, or mode transitions.
- Keep text legible under Dynamic Type, dark mode, and narrow device widths.
- Prefer system controls for settings, menus, pickers, toggles, sharing, and document import unless custom UI materially improves the workflow.
- Keep overlays mutually exclusive and predictable. Reader controls, menus, selection handles, and transient toasts should not fight for the same tap zone.
- Use system symbols and platform conventions before inventing custom controls.

## SwiftUI View Refactoring

- Order view files consistently: environment, immutable inputs, state, non-view computed values, `init`, `body`, view builders, then helper/async methods.
- Extract meaningful dedicated `View` types for non-trivial sections instead of hiding a large screen behind many computed `some View` properties.
- Keep computed `some View` helpers small and local. Move reusable, stateful, or independently meaningful subviews into dedicated types or files.
- Move non-trivial button actions, `.task`, `.onChange`, `.refreshable`, and side effects into named private methods. Keep `body` readable as UI.
- Avoid top-level `if/else` branches that swap the whole root view. Prefer a stable base view with conditional sections, overlays, toolbar items, opacity, or disabled state.
- Do not introduce a view model just to mirror local state or wrap environment dependencies.
- If a SwiftUI view file grows past roughly 300 lines, split it aggressively by user-facing sections or reusable controls.
- Keep behavior intact during refactors unless the user explicitly asks for behavior changes.

## Concurrency Safety

- Preserve behavior while fixing concurrency warnings; prefer the smallest safe isolation change.
- Keep UI state and observable stores on the main actor.
- Do not silence diagnostics with `@unchecked Sendable`, `nonisolated(unsafe)`, or broad `@preconcurrency` imports unless there is a documented reason.
- Move CPU-heavy parsing, thumbnail generation, indexing, and export work off the main actor, then marshal only UI updates back to the main actor.
- Respect task cancellation for imports, network fetches, rendering, search indexing, and export flows.
- Avoid unstructured detached tasks unless the work truly must outlive the calling context.

Use `my-ios-app-swift-concurrency` for strict concurrency diagnostics, `Sendable`, actors, `@MainActor`, task groups, and migration to modern Swift concurrency.

## Performance

- Keep heavy work out of `body`: parsing, sorting, formatting allocation, image decoding, PDF work, HTML generation, and file I/O belong in stores/helpers.
- Stabilize identity in `ForEach`, lists, grids, and reader pages. Never create `UUID()` inside render paths.
- Scope observable state narrowly so unrelated changes do not invalidate large view trees.
- Downsample large images and generated covers before display.
- Cache derived render data separately from canonical user data, with clear invalidation rules.
- Prefer lazy containers for large collections, but make item sizing stable to avoid layout churn.
- Profile release builds on a real device when code review cannot explain jank.

Use `my-ios-app-swiftui-performance` for detailed SwiftUI invalidation, identity, layout, image, and Instruments workflows.

## Accessibility

- Every interactive control needs a useful accessible label; icon-only buttons need explicit labels.
- Prefer real `Button`, `Toggle`, `Slider`, `Picker`, `Menu`, and `TextField` controls so traits and input behavior come for free.
- Custom controls must expose correct traits, values, hints, and adjustable actions where appropriate.
- Tap targets should be at least 44x44 points.
- Support Dynamic Type with adaptive layouts and system fonts where practical.
- Do not convey state by color alone.
- Respect Reduce Motion, Reduce Transparency, Bold Text, and Increase Contrast.
- When sheets, dialogs, or popovers close, restore focus to the triggering control when possible.

Use `my-ios-app-ios-accessibility` for VoiceOver, Voice Control, Switch Control, keyboard access, custom rotors, nutrition labels, and accessibility audits.

## Import, Share, And Files

For local import flows:

1. Let the user choose files with `UIDocumentPickerViewController` or app document-opening support.
2. Copy security-scoped files into app-managed storage before parsing.
3. Compute a content hash and detect duplicates before creating new records.
4. Prefer the source URL filename for the initial display title; fall back to embedded metadata when needed.
5. Detect file type from both extension and content signature when possible.
6. Parse metadata and generate thumbnails/covers in helper/parser files.
7. Save model metadata and refresh the UI through the app store/controller.

For sharing:

- Share a temporary exported copy when iOS APIs or receiving apps require a stable URL/name.
- Put share copies in a dedicated temporary folder so cache cleanup can remove them safely later.
- Do not delete a share file synchronously before the share sheet completes.

## Reader Rendering Surfaces

- Pick the narrowest rendering surface that matches the document format and required interactions.
- Use native SwiftUI for app chrome, library screens, settings, and simple text experiences.
- Use PDFKit for PDF display, navigation, search, selection, thumbnails, and annotations.
- Use WebKit for app-owned HTML, EPUB-style reflowable rendering, JavaScript-backed layout, or custom local URL schemes.
- Keep renderer-specific state behind a focused adapter/helper so feature views do not depend on file paths, raw HTML, or PDF internals.
- Preserve reading progress with stable document identifiers and renderer-specific positions.
- Avoid mixing multiple renderers in one view unless there is a clear abstraction boundary.

Use `my-ios-app-pdfkit` for PDF-specific implementation and `my-ios-app-swiftui-webkit` for WebKit or app-owned HTML rendering.

## Testing

- Add tests when changing parsers, persistence, migration, duplicate detection, progress tracking, search, export, or renderer adapters.
- Prefer Swift Testing for new unit tests in modern Swift projects.
- Keep XCTest for UI tests, performance tests, and snapshot test infrastructure when the project already uses it.
- Test model/helper behavior more than view structure.
- Use in-memory stores and temporary directories for persistence tests.
- Include at least one regression test for bug fixes that touch data loss, import, deletion, or reading progress.

Use `my-ios-app-swift-testing` for `@Test`, `#expect`, `#require`, async tests, migration from XCTest assertions, and modern test organization.

## Settings And Maintenance

- Keep settings screens independent from feature views.
- Group settings by user intent, not implementation detail.
- Put cache cleanup behind a clear confirmation sheet/action.
- Cache cleanup should delete derived or temporary data only, not canonical user data.
- When adding external links or app URL schemes, update `Info.plist` only for schemes actually used by `canOpenURL`.
- Make destructive actions explicit, reversible where practical, and scoped to the selected data.
- Keep diagnostics, debug toggles, and maintenance actions out of the main reading flow.

## App Store Metadata And ASO

Treat App Store metadata as a product surface, not a form-filling chore. The app name, subtitle, keywords, promotional text, description, icon, previews, and screenshots affect discoverability, ranking signals, first impression, and conversion.

### Metadata Strategy

- Start from user intent. List the search phrases a target user would naturally type when looking for this app, then map those phrases into name, subtitle, and keywords.
- Before generating app name, subtitle, promotional text, description, or keywords, collect competitor data when available. Use competitor listings, ASO dashboards, keyword rankings, reviews, screenshots, and category charts to avoid writing metadata from intuition alone.
- Balance search coverage with trust. Keyword stuffing can make the listing look low quality even when it increases matching surface.
- Keep metadata accurate. Do not promise unsupported features, unsupported platforms, pricing terms, celebrity names, trademarked terms, or competitor names unless there is a clear legal and review-safe reason.
- Localize metadata intentionally. Do not blindly translate keywords; choose terms people in that locale actually search for.
- Review metadata whenever product positioning changes, not only when code changes.

### Competitor Analysis Data Sources

Use ASO and app-intelligence tools to ground metadata decisions in observed market data. Do not rely only on intuition.

- **QiMai / 七麦数据**: use for China-focused App Store and Android market research, ranking snapshots, keyword coverage, keyword heat, Apple Ads/Search Ads signals, and competitor monitoring.
- **ASOTools**: use for overseas ASO, Google Play + App Store keyword research, competitor keyword ideas, country/category rankings, download and revenue estimates, and multi-locale analysis.
- **DianDian / 点点数据**: use for broader app/game intelligence, app rankings, market share, download/revenue trend estimates, user behavior signals, competitor comparison, and global market tracking.
- **ChanMaster / 蝉大师**: use for keyword expansion, keyword rank/coverage checks, ASO diagnosis, App Store ranking history, ASM/Apple Search Ads auxiliary analysis, and competitor ASO comparison.
- **ASO114**: use as a supplementary domestic ASO source for keyword coverage, ranking trends, Android channel checks, and competitor keyword gap analysis.

Treat third-party metrics as directional estimates, not absolute truth. Different vendors may disagree because their data models, market coverage, refresh cadence, and estimation methods differ.

### Competitor Analysis Workflow

1. Define the competitor set:
   - Direct competitors: apps solving the same user job.
   - Search competitors: apps ranking for the keywords you want.
   - Category competitors: apps consistently appearing in the same category charts.
   - Aspirational competitors: apps with strong conversion assets or positioning, even if the feature set differs.
2. Capture a metadata snapshot for each competitor:
   - App name, subtitle, keywords if visible or estimated, promotional text, description opening, screenshots, preview videos, category, rating, review count, version cadence, pricing, and IAP/subscription model.
3. Capture ASO signals:
   - Keyword coverage, keyword rank, keyword heat/search volume, ranking history, category rank, hot-search presence, download/revenue estimates, review trend, rating trend, and regional differences.
4. Identify gaps:
   - Keywords competitors rank for but this app does not.
   - High-intent long-tail terms with moderate difficulty.
   - Positioning claims competitors repeat.
   - Visual promises made by screenshots and previews.
   - Review complaints that reveal unmet user needs.
5. Convert findings into metadata decisions:
   - Put the strongest intent terms into app name and subtitle.
   - Put remaining relevant terms into keywords.
   - Use promotional text and description to answer the most common user hesitation.
   - Use screenshots and previews to prove the highest-value claims.
6. Track before/after:
   - Save the old metadata, new metadata, change date, target keywords, baseline ranks, and follow-up ranks.
   - Re-check after enough time has passed for indexing and ranking movement.

### Metadata Generation Workflow

When asked to generate app name, subtitle, promotional text, description, or keywords:

1. Collect inputs:
   - App purpose, target users, core features, monetization model, supported regions/languages, and any hard product claims.
   - Existing app metadata, if any.
   - Competitor list, target keywords, category, and country/region.
2. Gather competitor evidence:
   - Use ASO tools, App Store pages, public listings, or user-provided exports/screenshots to collect competitor names, subtitles, description openings, screenshot promises, review complaints, keyword coverage, and rank signals.
   - If live data access is unavailable, ask for tool exports or competitor URLs; otherwise state that the metadata draft is based on provided context only.
3. Build a keyword pool:
   - Separate brand terms, category terms, feature terms, problem terms, audience terms, and long-tail intent terms.
   - Mark each term as high/medium/low priority based on relevance, competitor usage, search intent, and likely difficulty.
4. Allocate words deliberately:
   - App name: brand plus the strongest core term if it remains natural.
   - Subtitle: primary value proposition plus secondary high-intent terms.
   - Keywords: relevant remaining terms, synonyms, and long-tail fragments not already covered by name/subtitle.
   - Promotional text: timely value proposition or campaign copy aimed at conversion.
   - Description: benefit-first explanation, proof points, feature bullets, and trust-building details.
5. Return options with rationale:
   - Provide 3-5 candidate name/subtitle combinations when naming is requested.
   - Provide a keyword list grouped by intent, not just one comma-separated dump.
   - Explain which competitor or search-intent evidence influenced the choices.
   - Call out unsupported assumptions and review-risky claims.

### Data Collection Rules

- Prefer official exports, paid dashboard downloads, public pages, or documented APIs when available.
- Do not bypass login, paywalls, rate limits, anti-bot controls, robots.txt, or platform terms just to scrape data.
- If using screenshots or manual notes from paid tools, record the source, query date, country/region, platform, and selected category.
- Normalize data before comparing competitors: same country, same device family, same app store, same category, and same date range.
- Keep a simple competitor matrix so metadata changes are explainable later.

### App Name

- Keep the app name simple, memorable, easy to spell, and connected to what the app does.
- Use the pattern `Brand/App Name + highest-value core keyword` only when it still reads naturally.
- Put the strongest, most differentiated keyword here; do not waste the name on generic category words alone.
- Stay within App Store limits and avoid names that are too similar to existing apps.

### Subtitle

- Use the subtitle to complete the app name: explain the primary value, core use case, or audience.
- Add secondary high-intent keywords that were not already used in the name.
- Avoid generic claims such as "best app" or vague emotional copy with no feature signal.
- Treat name and subtitle as one combined sentence users see in search and on the product page.

### Keywords

- Use the keyword field for relevant search terms not already covered by the app name or subtitle.
- Prefer concise single terms separated by commas without spaces so the character budget goes to searchable words.
- Mix broad category terms with specific use-case terms. Broad terms increase reach; specific terms usually express stronger intent.
- Avoid duplicates across name, subtitle, and keywords unless testing proves a reason.
- Include common synonyms, feature terms, audience terms, and problem terms. Skip words that are irrelevant just because they have high volume.
- Track performance and revise keywords over releases; ASO is an iterative process.

### Promotional Text

- Use promotional text for timely conversion copy: new features, seasonal campaigns, limited-time positioning, or a strong current value proposition.
- Keep it self-contained and useful even if the user reads only the first sentence.
- Do not treat promotional text as the main keyword field; use it to persuade visitors who already reached the product page.
- Prefer specific benefit copy over hype.

### Description

- Lead with the clearest user benefit and the problem the app solves.
- Use short paragraphs and scannable feature bullets. Put the most important information before the user has to expand or keep reading.
- Explain what makes the app different, who it is for, and what the user can do immediately after installing.
- Keep claims review-safe and supportable. Avoid unverifiable superlatives, roadmap promises, and misleading pricing language.
- Refresh the description when major features, pricing, subscription behavior, or positioning changes.

### Review Checklist

- Does the name/subtitle/keyword set cover the target search intents without obvious duplication?
- Does the listing read like a trustworthy product page, not an SEO dump?
- Are the first visible words of the name, subtitle, promotional text, and description understandable without context?
- Are all claims true in the current app build?
- Are metadata, screenshots, and app behavior aligned?

## Release Notes

- Generate release notes from actual git history, tags, or an explicit ref range.
- Keep App Store copy user-facing: describe visible benefits and fixes, not internal commits, refactors, CI, or dependency churn.
- Every release note bullet should map back to a real change in the selected range.
- Prefer concise bullets grouped by user value when the release has many changes.

Use `my-ios-app-store-changelog` for App Store "What's New", TestFlight notes, and user-facing changelog generation.

## Specialized Skill Routing

- Use this skill first for app-level direction, code organization, and implementation posture.
- Use `my-ios-app-swiftui-patterns` for detailed SwiftUI state, environment, composition, and app structure.
- Use `my-ios-app-swift-concurrency` for strict concurrency diagnostics, actors, `Sendable`, and task behavior.
- Use `my-ios-app-swift-testing` for Swift Testing syntax, XCTest migration, and test design details.
- Use `my-ios-app-swiftui-performance` for SwiftUI jank, invalidation, identity churn, layout thrash, and profiling.
- Use `my-ios-app-ios-accessibility` for accessibility implementation or audits.
- Use `my-ios-app-swiftdata` for SwiftData/Core Data persistence details.
- Use `my-ios-app-pdfkit` for PDF rendering, search, thumbnails, annotations, and SwiftUI wrapping.
- Use `my-ios-app-swiftui-webkit` for WebKit-backed HTML, EPUB-style rendering, JavaScript, local content, or custom URL schemes.
- Use `my-ios-app-store-changelog` for App Store "What's New", TestFlight notes, or release changelog copy.

## Validation

For SwiftUI iOS apps, run a generic device build to avoid simulator availability issues:

```bash
xcodebuild -project <AppName>.xcodeproj -scheme <AppName> -destination generic/platform=iOS -derivedDataPath /private/tmp/<AppName>_derived CODE_SIGNING_ALLOWED=NO build
```

If the sandbox blocks Swift preview macros or Xcode plugin tooling, rerun the same command with approval outside the sandbox.
