# Stage 1 — Project context (read-only)

Goal: know the project well enough that the SPEC is written against reality.
Read this file at the start of Stage 1. Work through the sections in order;
skip what a saved project context already covers (1.0). Nothing in this stage
edits source code.

## 1.0 Reuse saved context

Look for a saved project context: `PROJECT_CONTEXT.md` in the feature docs
folder (default `docs/features/PROJECT_CONTEXT.md`), or the equivalent of a
spec-driven tool already in use (Spec Kit constitution, Kiro steering files,
OpenSpec `project.md`). If it exists:

- Read it and spot-check the facts this feature depends on (build files,
  modules, the flows it touches). Refresh only what changed.
- Skip the inspection steps below that it already covers, and say so in the
  context report.

If it does not exist, do the full Stage 1 and, at the end, offer to save the
stable part (platforms, architecture, commands, conventions, mobile baseline)
using `references/project-context-template.md`. Write it only if the user
agrees. Feature-specific findings stay in the SPEC.

## 1.1 Project instructions

Look for, and read, whichever exist: `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`,
`.cursor/rules/*` (`.mdc`), `.cursorrules`, `.windsurfrules`, `.clinerules`,
`.github/copilot-instructions.md`, `.github/instructions/*.instructions.md`,
`.junie/guidelines.md`, `CONTRIBUTING.md`, `README.md`, and any files they
declare as required reading. Follow `@file` imports and "read X first" links
explicitly — do not assume your tool imports them. For large files, read the
headings first, then the sections relevant to the feature (architecture,
conventions, current behavior, workflow, known debt, commands).

## 1.2 Existing spec-driven material

Search `docs/`, `specs/`, `.specs/`, `documentation/` and similar, plus the
folders of spec-driven tools:

| Found | Tool | What to do |
| --- | --- | --- |
| `.specify/`, `specs/NNN-<name>/spec.md` | GitHub Spec Kit | Use its folder layout, numbering, templates and constitution |
| `.kiro/specs/<name>/` (`requirements.md`, `design.md`, `tasks.md`), `.kiro/steering/` | Kiro | Map SPEC → `requirements.md`, PLAN → `design.md`, TASKS → `tasks.md`; keep its format |
| `openspec/` (`project.md`, `changes/`, `specs/`) | OpenSpec | Write the feature as a change in its layout |

When one of these is in use, keep its file names and structure and add the
SDMD content (mobile behavior, per-platform evidence) inside them. Otherwise:

- Templates (`SPEC_TEMPLATE`, `PLAN_TEMPLATE`, `TASKS_TEMPLATE` or equivalents)
  → if they exist, use them instead of this skill's templates, preserving their
  structure and embedded comments.
- Mobile guidelines (`MOBILE_GUIDELINES.md` or similar) → merge with
  `references/mobile-guidelines.md`; project items win, skill items fill gaps.
- Previous feature folders → learn the folder convention, the ID scheme and the
  expected depth. Read one approved SPEC as a style reference.
- If no convention exists, propose `docs/features/<feature-name>/` with
  `SPEC.md`, `PLAN.md`, `TASKS.md` (or `FEATURE.md` on the lite track), and
  confirm the folder name with the user.

## 1.3 Detect the platform(s)

Check the rows **in this order**; the first framework row that matches
classifies the app.

| # | Signal in the repository | Platform | Read |
| --- | --- | --- | --- |
| 1 | `pubspec.yaml` declaring the `flutter` SDK | Flutter | `platforms/flutter.md` |
| 2 | `package.json` depending on `react-native` or `expo` | React Native | `platforms/react-native.md` |
| 3 | Gradle with the Kotlin `multiplatform` plugin, `commonMain` source set | Kotlin Multiplatform | `platforms/kmp.md` |
| 4 | Gradle with `com.android.application` (or `com.android.library` for an SDK) and none of the above | Android native | `platforms/android.md` |
| 5 | `*.xcodeproj`, `*.xcworkspace`, `Package.swift`, `Project.swift` (Tuist) or `project.yml` (XcodeGen), and none of rows 1–3 | iOS native | `platforms/ios.md` |

- In Flutter and React Native projects the `android/` and `ios/` folders are
  **host projects**, not separate native apps; in KMP, `iosApp/` is the iOS
  host. Load `android.md` and `ios.md` for host details (permissions,
  manifest, Info.plist, safe areas), but classify the app by its framework.
- A repository can contain several apps (e.g. a monorepo with a native Android
  app and a native iOS app, or KMP with a native SwiftUI app). Classify each
  one and load every matching file.
- **Unrecognised technology** (e.g. .NET MAUI, Ionic/Capacitor, NativeScript,
  Objective-C only, a custom engine): tell the user there is no platform file
  for it, work from `mobile-guidelines.md` and the host files that still
  apply, and take every command and convention from the repository or the
  user — never from assumption.
- **Target platforms:** record which platforms the app ships on, from build
  targets, store configuration and the user's confirmation (a Flutter app may
  ship only on Android; a native iOS app may also run on iPad). This list
  drives every per-platform row from here on.

## 1.4 Inspect the code

Using the platform file's checklist, establish and note with file paths:

- **Architecture:** layers or modules, dependency direction, where state lives,
  DI (or its absence), navigation mechanism, design system, reference
  implementations the project points to.
- **Business flow:** the screens and flows the feature touches, what is real
  behavior versus sample/mock data versus empty stubs.
- **Data:** networking, API contract sources (OpenAPI, DTOs, docs), persistence,
  caching, secure storage, configuration/environments.
- **Tests:** frameworks and libraries actually on the test classpath, where tests
  live, what is covered and what is not (UI, navigation, platform code), and
  whether the suite is green if the docs say so.
- **Commands:** build, test and lint commands — never guessed. Sources, in
  order of trust: project docs; CI configuration (`.github/workflows/`,
  `.gitlab-ci.yml`, `bitrise.yml`, `codemagic.yaml`, `eas.json`, Xcode Cloud
  scripts); `fastlane/Fastfile`; `Makefile` / `justfile`; `package.json`
  scripts; build configuration. Include the **release** build command of each
  target and whether minification/obfuscation is on. Note which require
  macOS/Xcode, signing, a device, an emulator or a simulator.
- **Conventions and constraints:** dependency policy, naming and language of
  identifiers, formatting tools, forbidden APIs, known debt, git workflow
  (branch naming, commit format, PR template) if documented.

## 1.5 Mobile baseline

Write a short list of **facts the code establishes today** for each mobile
concern the feature may touch (lifecycle, state restoration, back navigation,
orientation, keyboard, safe areas/insets, theming/dark mode, accessibility,
permissions, offline and cache, persistence, secure storage, logging,
analytics, localization and formats, release build configuration such as
minification and keep rules, and store privacy declarations kept in the repo). Each fact cites the file that proves
it. Purpose: a SPEC may not silently *inherit* a behavior the app does not
have; if the feature needs it, the SPEC must define it.

## 1.6 Report the context

Give the user a concise summary: platform(s) and target platforms,
architecture in a few lines, the flows the feature touches, the test
situation, the relevant baseline facts, and risks or conflicts found. Then
move to Stage 2. Do not paste large file dumps.

## 1.G New project (no code yet)

If there is no app in the repository yet, Stage 1 becomes a decision stage
instead of a discovery stage: agree with the user on target platforms,
technology, architecture, state management, navigation, DI, persistence,
testing approach, commands and conventions — one batch of questions at a
time, with a recommendation each. Record the answers as **Confirmed** in a
`PROJECT_CONTEXT.md` (from `references/project-context-template.md`), get it
approved, and then write the first SPEC against it. The project skeleton is
itself a feature: it goes through SPEC, PLAN and TASKS like any other.
