# Changelog

All notable changes to this project are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project uses
[Semantic Versioning](https://semver.org/).

## [1.2.0] — 2026-10-02

Based on feedback from early users.

### Changed

- **Lighter `SKILL.md`** (about half the size): it keeps the stages, rules and
  gates; Stage 1 in detail moved to `references/context-discovery.md`, and the
  SPEC section list lives only in the template.
- **Fewer, better approvals:** a *combined* approval mode (PLAN and TASKS
  approved together), approval and authorization in one explicit reply, and
  no sign-off question when every check passed. The SPEC gate always stays.
  Every approval request comes with a review summary of at most six bullets.
- **Shorter documents:** the SPEC includes only the mobile behavior rows and
  sections that apply; the rest are listed in one line. The SPEC template
  merges acceptance criteria with how they are checked.
- Lite track: approval of Part 2 and authorization can come in one reply;
  "no new network call" replaces "no new contract" as a condition.
- Status line shows the approval mode (`full/step`, `full/combined`, `lite`).

### Added

- **Release-build check** in the evidence rules when a feature touches
  serialization, reflection, code generation, native code, build configuration
  or a new/updated library, with per-platform guidance (R8/ProGuard and
  `mapping.txt` on Android, Release configuration on iOS, Flutter release and
  obfuscation, React Native release bundle, KMP release framework).
- **Store privacy declarations:** Google Play Data safety form and restricted
  permission declarations next to App Store privacy labels and the privacy
  manifest; console forms are tracked as release items outside the repository.
- Lite-track example with native Android and iOS apps.
- `benchmark/`: protocol and template to compare the same real feature with
  and without the skill.

## [1.1.0] — 2026-09-30

First public release.

### Added

- **Target platforms:** Stage 1 records the platforms the app ships on; every
  per-platform row, validation row and parity question uses only those.
  Single-platform apps no longer get Android/iOS rows that do not apply.
- **Lite track** for small features (`references/lite-template.md`): one
  document, two approvals, then a separate authorization.
- **Reusable project context** (`references/project-context-template.md`),
  saved with the user's agreement and spot-checked on each feature.
- **Resuming** a feature from the status of its documents.
- **Unattended runs:** stop after the SPEC draft with open questions.
- **New projects (greenfield):** Stage 1 becomes a decision stage.
- Detection of **GitHub Spec Kit, Kiro and OpenSpec** layouts.
- More instruction files read in Stage 1 (`.cursor/rules/*.mdc`,
  `.windsurfrules`, `.clinerules`, `.github/instructions/`, `.junie/`).
- Commands taken from **CI configuration**, fastlane, Makefile/justfile and
  `package.json` scripts.
- Requirements as **links** (tickets, issues, designs).
- SPEC sections **Design references** and **Analytics and feature flags**.
- Guideline points **Theming and appearance** (dark mode, contrast), reduced
  motion, analytics and feature flags; matching notes in each platform file.
- **Git** guidance: no commits, branches or PRs unless asked.
- `platforms/_template.md` for contributors.
- README (English and Spanish), INSTALL guide for each agent, CONTRIBUTING,
  LICENSE, Claude Code plugin and marketplace manifests, a complete example.

### Changed

- Default IDs are now **FR** (functional requirement) and **AC** (acceptance
  criterion). Projects that already use another scheme (e.g. `RF`/`CA`) keep it.
- Platform detection has an explicit **precedence**: Flutter, React Native and
  KMP are detected before native; their `android/` and `ios/` folders are
  treated as host projects.
- Unrecognised technologies fall back to the generic guidelines instead of
  guessing.
- The mobile behavior table has a **single source** (`mobile-guidelines.md`);
  the SPEC template references it instead of duplicating it.
- React Native: the New Architecture is described as version-dependent
  instead of optional.
- Skill moved to `skills/spec-driven-mobile-development/`.

### Fixed

- Stage count and gate wording in `SKILL.md` (stages 0–1 end with a
  confirmation or report, not an approval).

## [1.0.0] — 2026-09-28

- Initial version (private use).
