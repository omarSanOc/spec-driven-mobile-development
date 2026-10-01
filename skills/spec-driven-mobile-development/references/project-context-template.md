# PROJECT CONTEXT

**Last verified:** [YYYY-MM-DD] · [commit hash if available]
**Status:** Draft <!-- Draft | Approved (YYYY-MM-DD) -->

<!-- FOR THE AGENT
- Stable facts about the project that every feature reuses. Feature-specific
  findings belong in each SPEC, not here.
- Every fact cites the file that proves it (existing project) or is marked
  Confirmed (decision by the user, e.g. a new project). No guesses.
- Write it only with the user's agreement. When a feature changes a fact
  recorded here, propose the update at the end of that feature.
- Before relying on it, spot-check the facts the current feature depends on
  and refresh what changed.
- If the project already has a Spec Kit constitution, Kiro steering files or an
  OpenSpec project.md, extend that file instead of creating this one.
-->

## Platforms

- **Technology:** [Android native / iOS native / KMP / Flutter / React Native / other] — [evidence]
- **Target platforms:** [Android, iOS, iPadOS, tablets…] — [evidence]
- **Minimum OS / SDK versions:** [values] — [evidence]

## Architecture

- **Modules / layers and dependency direction:** [ ]
- **State management:** [ ]
- **Navigation:** [ ]
- **DI:** [ ]
- **Design system / theming:** [ ]
- **Reference feature to imitate:** [path]

## Data

- **Networking and API contract source:** [ ]
- **Persistence and cache:** [ ]
- **Secure storage:** [ ]
- **Environments and configuration:** [ ]

## Tests

- **Frameworks on the test classpath:** [ ]
- **Where tests live / what is covered:** [ ]

## Commands (verified)

| Purpose | Command | Requires |
| --- | --- | --- |
| Build [platform] | | |
| Unit tests | | |
| Lint / format | | |

## Conventions

- **Feature docs folder and ID scheme:** [e.g. docs/features/<name>/, FR/AC]
- **Naming, language of identifiers, formatting:** [ ]
- **Dependency policy:** [ ]
- **Git workflow (branches, commits, PR template):** [ ]
- **Known debt and forbidden APIs:** [ ]

## Mobile baseline

<!-- What the app does today for each concern; cite the file. "Not handled"
is a valid, useful fact. -->

| Concern | Current behavior | Evidence |
| --- | --- | --- |
| Lifecycle / process death | | |
| State restoration | | |
| Back navigation | | |
| Orientation / screen sizes | | |
| Insets / safe areas / keyboard | | |
| Theming / dark mode | | |
| Accessibility | | |
| Permissions | | |
| Offline / cache | | |
| Logging / analytics / crash reporting | | |
| Localization and formats | | |
