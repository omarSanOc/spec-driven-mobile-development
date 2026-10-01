---
name: spec-driven-mobile-development
description: Spec-Driven Mobile Development (SDMD) for iOS, Android, Kotlin Multiplatform, Flutter and React Native apps. Use when the user hands over new requirements, user stories, a ticket or a feature request, in any language (e.g. new requirements, feature spec, user story, nuevos requerimientos, nueva funcionalidad, historia de usuario), and wants them turned into an approved SPEC, PLAN and TASKS before any code is written, then implemented and validated with per-platform evidence. Also use to resume a feature that already has SDMD documents. Not for one-line fixes or general questions.
license: Apache-2.0
compatibility: Works with any agent that can read the repository. Building and testing need each platform's toolchain (Xcode on macOS for iOS); checks that cannot run are reported as BLOCKED.
metadata:
  author: Omar Sánchez
  version: "1.1.0"
  repository: https://github.com/omarSanOc/spec-driven-mobile-development
---

# Spec-Driven Mobile Development (SDMD)

The user gives you **only the new requirements**. Everything else — architecture,
conventions, business flow, tests, platform differences — you discover from the
repository. You then take the requirements through these stages:

```
0 Intake → 1 Context → 2 SPEC ✔ → 3 PLAN ✔ → 4 TASKS ✔ → 5 Implementation ✔ → 6 Validation ✔
 (confirm   (report)   (approval)  (approval)  (approval)   (authorization)      (sign-off)
  grouping
  and track)
```

Stages 0 and 1 end with a confirmation or a report. Stages 2–4 each have an
**explicit approval** gate; implementation also needs a separate authorization,
and Stage 6 ends with the user's sign-off.

This file is tool-agnostic. "Ask the user" means whatever mechanism your
environment offers (a structured question tool if you have one, otherwise a
plain message). "Read a file" means whatever file access you have. If you cannot
run commands, say which checks you could not run instead of pretending.

Supporting files (read them when the stage says so):

| File | When |
| --- | --- |
| `references/mobile-guidelines.md` | Stage 2 onward; keep it open until validation |
| `references/platforms/<platform>.md` | Stage 1, after detecting the platform(s) |
| `references/project-context-template.md` | Stage 1, to save reusable project context |
| `references/spec-template.md` | Stage 2, if the project has no SPEC template |
| `references/plan-template.md` | Stage 3, if the project has no PLAN template |
| `references/tasks-template.md` | Stage 4, if the project has no TASKS template |
| `references/lite-template.md` | Lite track only (see "Tracks") |
| `references/evidence-rules.md` | Stages 3, 5 and 6 |

---

## Non-negotiable rules

1. **No production code before authorization.** Stages 0–4 are read-only for
   source code: you only create or edit the feature documents. Implementation
   starts only when the user explicitly authorizes it. A document marked
   "Approved" is not an authorization to implement.
2. **Never invent.** Every statement in a document is one of:
   - **Verified** — you read it in the code or docs; cite the path.
   - **Confirmed** — the user decided it in this conversation; record it as a decision.
   - **Proposed** — your suggestion; labelled as such until the user confirms it.
   - **PENDING** — unknown or undecided; stays visibly marked.
   Do not invent requirements, exclusions, files, APIs, versions, commands,
   test results or capabilities.
3. **Propose, recommend, wait.** You may offer options and recommend one, but a
   proposal becomes a decision only after the user confirms it. A product
   question is never answered by a technical assumption.
4. **Few questions at a time.** At most three per turn, highest impact first.
   Each question gives 2–4 options, marks one as recommended, and says why.
   After each answer, update the document before asking the next batch.
5. **Precedence.** User's explicit instructions > the project's own rules
   (`AGENTS.md`, `CLAUDE.md`, guidelines, templates, conventions of a
   spec-driven tool already in use) > this skill's defaults. Where a project
   document disagrees with this skill, the project wins; where project
   documents disagree with each other in a way that changes behavior or scope,
   ask before proceeding.
6. **Guidelines do not widen scope.** Mobile guidelines tell you what to
   *consider*; only the items the feature touches become requirements, and
   only after the user agrees. Items that do not apply are recorded as `N/A`
   with a reason.
7. **Evidence per platform.** Every result is recorded for each **target
   platform** (the platforms the app ships on, established in Stage 1).
   Something working on one platform is not evidence for another. A blocked
   check is reported as BLOCKED, never as passed. See
   `references/evidence-rules.md`.
8. **Language.** Talk to the user in their language. Write feature documents in
   the language the project's existing feature docs use; if there are none, in
   the user's language. Write code, identifiers and comments following the
   repository's existing convention, whatever language that is.

### Identifiers

Default IDs: `FR-01` functional requirement, `AC-01` acceptance criterion,
`D-01` decision, `UX-01` UI/UX suggestion, `T-01` task. If the project already
uses another scheme (e.g. `RF`/`CA`, `REQ`, Spec Kit or Kiro numbering), use
that scheme instead and keep it consistent across SPEC, PLAN and TASKS.

### Target platforms

Stage 1 records the **target platforms**: the platforms this app ships on
(e.g. Android only; iOS only; Android + iOS). Everything per-platform in this
skill — mobile behavior rows, validation rows, evidence, parity — uses that
list. Rows about a platform that is not a target are omitted, and platform
parity questions exist only when there is more than one target.

---

## Tracks

At the end of Stage 0, propose a track and let the user confirm it.

- **Full track** (default): SPEC, PLAN and TASKS as separate documents, with
  every gate below.
- **Lite track**, for small features: one `FEATURE.md` from
  `references/lite-template.md` with two gates — (1) approve the spec part,
  (2) approve the plan and tasks part, then a separate explicit
  authorization to implement. Offer it only when **all** of these hold: one
  flow or screen; no new dependency; no new or changed network contract; no
  persistence or schema change; no new permission or entitlement; roughly five
  tasks or fewer. If any of these stops holding, say so and move to the full
  track, carrying over what was already approved.

The non-negotiable rules apply to both tracks.

---

## Resuming a feature

Before starting Stage 0, check whether the requirements belong to a feature
that already has documents (same name, linked ticket, or the user says so).
If it does:

1. Read its SPEC, PLAN, TASKS (or `FEATURE.md`) and their **Status** fields.
2. Resume at the first stage that is not approved; if implementation was
   authorized, resume at the first unticked task.
3. Re-read the project context (Stage 1.0). If the code changed since the
   documents were approved in a way that affects them, report it and treat it
   as a change after approval (see the end of this file).
4. Report where you are with the status line and continue. Do not repeat
   approved stages or ask again for decisions already recorded.

## When nobody is there to approve

If you run without an interactive user (for example an agent triggered from
an issue or a CI job), do Stages 0–2 only: write the SPEC draft, put every
open question in its "Decisions" table with options and a recommendation,
set its status to **Draft — waiting for answers**, and stop. Never treat
silence as approval or authorization.

---

## Stage 0 — Intake

Goal: understand what was asked before touching the repository.

1. If the requirements arrive as links (Jira, Linear, GitHub/GitLab issue,
   Figma, a document), read them with the tools you have. If you cannot open
   a link, ask the user to paste the content; never guess it.
2. Restate the requirements as a numbered list, in your own words, without
   adding anything. Keep the user's original text available for reference.
3. Flag, without resolving: ambiguous terms, missing actors or triggers,
   contradictions between requirements, and anything that sounds like a
   solution instead of a need.
4. If there are several requirements, propose how to group them into features
   (one SPEC per coherent feature) and ask the user to confirm the grouping.
5. Propose the track (full or lite) with a one-line reason.
6. Do not ask about anything the repository might answer. Hold those questions
   until Stage 1 is done.

---

## Stage 1 — Project context (read-only)

Goal: know the project well enough that the SPEC is written against reality.

### 1.0 Reuse saved context

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

### 1.1 Project instructions

Look for, and read, whichever exist: `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`,
`.cursor/rules/*` (`.mdc`), `.cursorrules`, `.windsurfrules`, `.clinerules`,
`.github/copilot-instructions.md`, `.github/instructions/*.instructions.md`,
`.junie/guidelines.md`, `CONTRIBUTING.md`, `README.md`, and any files they
declare as required reading. Follow `@file` imports and "read X first" links
explicitly — do not assume your tool imports them. For large files, read the
headings first, then the sections relevant to the feature (architecture,
conventions, current behavior, workflow, known debt, commands).

### 1.2 Existing spec-driven material

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

### 1.3 Detect the platform(s)

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

### 1.4 Inspect the code

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
  scripts; build configuration. Note which require macOS/Xcode, a device, an
  emulator or a simulator.
- **Conventions and constraints:** dependency policy, naming and language of
  identifiers, formatting tools, forbidden APIs, known debt, git workflow
  (branch naming, commit format, PR template) if documented.

### 1.5 Mobile baseline

Write a short list of **facts the code establishes today** for each mobile
concern the feature may touch (lifecycle, state restoration, back navigation,
orientation, keyboard, safe areas/insets, theming/dark mode, accessibility,
permissions, offline and cache, persistence, secure storage, logging,
analytics, localization and formats). Each fact cites the file that proves
it. Purpose: a SPEC may not silently *inherit* a behavior the app does not
have; if the feature needs it, the SPEC must define it.

### 1.6 Report the context

Give the user a concise summary: platform(s) and target platforms,
architecture in a few lines, the flows the feature touches, the test
situation, the relevant baseline facts, and risks or conflicts found. Then
move to Stage 2. Do not paste large file dumps.

### 1.G New project (no code yet)

If there is no app in the repository yet, Stage 1 becomes a decision stage
instead of a discovery stage: agree with the user on target platforms,
technology, architecture, state management, navigation, DI, persistence,
testing approach, commands and conventions — one batch of questions at a
time, with a recommendation each. Record the answers as **Confirmed** in a
`PROJECT_CONTEXT.md` (from `references/project-context-template.md`), get it
approved, and then write the first SPEC against it. The project skeleton is
itself a feature: it goes through SPEC, PLAN and TASKS like any other.

---

## Stage 2 — SPEC (what and why, not how)

Read `references/mobile-guidelines.md` in full (plus the project's guidelines).
Create the SPEC from the project template, or `references/spec-template.md`.
Status: **Draft**.

### 2.1 First draft

Fill everything you can **verify**; mark the rest PENDING. The SPEC must define:

- **Goal and expected behavior** — the need, who has it, what they will be able to do.
- **Current situation** — relevant existing behavior and what must be preserved
  (from Stage 1, with the baseline facts that matter).
- **Design references** — links to designs, screenshots or design-system
  components the user provided or the repo contains. Mark what is missing.
- **In scope** — requirements `FR-01…`, each observable and testable.
- **Out of scope** — only exclusions the user agreed to. Your suggested
  exclusions are listed as proposals.
- **User flows** — entry point, steps, outcome, affected screens, alternatives.
- **Business rules and data** — fields, validations, limits, ordering,
  duplicates, calculations, permissions by role. Meaning, not storage design.
- **Error cases** — each case the user must be able to tell apart, with what
  they see. Do not let a status code decide the message.
- **Mobile behavior table** — the table defined in `mobile-guidelines.md`
  (the single source for its rows), filled with an observable result per row,
  or `N/A` with its reason. Rows for non-target platforms are omitted; the
  platform-differences row exists only with more than one target platform
  ("None" when identical).
- **Service contract** — only if the feature talks to a network: endpoints,
  fields, error shapes, and which of those you verified versus assumed.
- **Analytics and feature flags** — only if the project uses them or the user
  asks: events and their properties, flag name and default. Otherwise `N/A`.
- **UI/UX suggestions** — improvements you noticed (accessibility, empty/error
  states, feedback, copy, consistency with the design system). Each is a
  **proposal** with a short rationale and is not a requirement until confirmed.
- **Constraints** — conditions already imposed (by the user or project rules):
  platforms, dependency policy, architecture boundaries, compatibility,
  measurable performance or accessibility targets. Your preferences are not
  constraints.
- **Acceptance criteria** — `AC-01 · FR-01: Given…, when…, then…` with an
  observable result. Never "works correctly". Include agreed alternative cases.
- **How each criterion is checked** — for every AC: the scenario, the expected
  result, whether it is provable by an automated unit test of shared/business
  logic or needs UI, device, emulator or simulator verification, and on which
  target platform(s). Tool choice is for the PLAN.
- **Pending decisions** — `D-01…`, each with options and your recommendation.

Keep in the SPEC **architecture constraints and fit** (e.g. "must follow the
existing unidirectional state pattern", "no new dependencies"), not class,
table, or file design — that belongs to the PLAN.

### 2.2 Question loop

Present the draft (or its link/path) and ask the 1–3 highest-impact questions.
Priority order: scope → business rules → error cases → mobile behavior →
acceptance criteria → UI/UX proposals. After each answer, update the SPEC
(move items from PENDING/Proposed to Confirmed, adjust affected sections) and
show what changed in one or two lines. Repeat.

### 2.3 Gate

Before requesting approval, check: scope agreed; exclusions agreed; flows
consistent; every FR has at least one AC; every AC has a check method and
target platform(s); every applicable guideline covered or `N/A` with reason;
no PENDING items left, or the user explicitly accepted the remaining ones as
deferred. Then ask: "Do you approve the SPEC?" Mark it **Approved** (with date)
only after an explicit yes. A complete document is not an approved one.

---

## Stage 3 — PLAN (how)

Read the approved SPEC, the project instructions, the guidelines, the platform
file(s) and `references/evidence-rules.md`. Use the project's PLAN template or
`references/plan-template.md`.

- **Verified technical context:** existing components and paths that take part.
  Reference only paths you verified; label new paths as proposed.
- **Solution:** approach and reasons; data and event flow to the UI; which
  layer/module/source set each change lives in; reuse before creating.
- **Platform-specific code:** every descent into platform code (`expect/actual`,
  platform channels, native modules, per-platform targets) justified, with
  every target platform's side planned in the same change.
- **Mobile decisions → mechanism:** for each mobile decision in the SPEC, where
  it lives and what implements it: state retention/restoration, cancellation of
  in-flight work, duplicate prevention, error and retry handling, local vs
  remote data, secure storage, back handling on each target platform.
- **Dependencies:** none by default. Any addition is a decision for the user,
  with purpose, alternatives, and proof it supports every target.
- **Validation strategy:** one row per AC and target platform — method (unit,
  integration, UI, manual), existing or proposed test, environment, expected
  evidence. Unit tests cover business rules, state transitions and every error
  case the SPEC named, using fakes at the boundaries.
- **Implementation order** with a compile/test checkpoint after each step.
- **Risks and pending decisions.**

If planning reveals a behavior or scope question, go back to the SPEC, ask, and
update it — do not resolve it inside the plan. Ask for explicit approval; mark
**Approved** only after a yes.

---

## Stage 4 — TASKS

Derive `TASKS.md` from the approved PLAN (project template or
`references/tasks-template.md`): small, ordered, verifiable tasks `T-01…`, each
with objective, scope, what it does *not* do, dependencies, the FR/AC it
serves, and its validation. Every task leaves the project compiling on every
target. End with the validation table (one row per AC and target platform,
empty result columns). Ask for approval, then ask separately:
**"Do you authorize implementation?"**

---

## Stage 5 — Implementation (only after explicit authorization)

- Execute tasks in order, within the approved scope and architecture.
- After each task: run its validation, record the observed result in
  `TASKS.md`, tick the box only if the validation was executed and passed.
- Follow the repository's conventions and existing patterns; no unrelated
  refactors, no dependency upgrades, no opportunistic fixes of known debt.
- Write tests alongside the code as the PLAN specifies; never weaken or delete
  a test to make it pass.
- If you find a requirement gap or contradiction: stop that part, explain it,
  propose options, update SPEC/PLAN/TASKS after the user decides, then continue.
- **Git:** do not create branches, commits or pull requests unless the user or
  the project's rules ask for it. When asked: follow the project's branch and
  commit conventions; otherwise one commit per task referencing its ID
  (e.g. `T-03: …`), and a PR description built from the SPEC goal, the FR
  list, the task list and the validation table. Never push or merge without
  being asked.
- Do not ask for renewed permission for steps already authorized.

---

## Stage 6 — Validation and completion report

- Fill the validation table: for each AC and target platform, the evidence and
  the result: PASSED, FAILED, NOT RUN or BLOCKED (with reason and the concrete
  check still needed).
- Run the full verification the project requires (build every target, unit
  tests on every target that runs them, lint if configured).
- Report to the user: what changed, documents updated, per-platform results,
  what remains unverified and why, and follow-up items. Ask for sign-off.
- If a `PROJECT_CONTEXT.md` exists and the feature changed something it
  records (a new module, command or baseline fact), propose the update.

---

## Every turn, start with a status line

```
SDMD · <feature> · <track> · Stage <n> <name> · <document> <status>
```

Then: what you did, what changed in the document, and (if any) the next 1–3
questions. Keep it short; the document holds the detail.

## Changes after approval

If the user changes a requirement after a stage was approved, identify every
affected FR, AC, plan section and task, update them, set the affected document
back to "In review", and request approval again before continuing.
