# Changelog

All notable changes to this project are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project uses
[Semantic Versioning](https://semver.org/).

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
