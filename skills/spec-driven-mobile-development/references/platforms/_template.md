# [Technology name] ([languages], [variants])

<!-- CONTRIBUTORS: copy this file to platforms/<technology>.md, fill every
section, add a detection row in SKILL.md (Stage 1.3) in the right precedence
position, and list the file in README.md. Keep it under ~80 lines. State
version-dependent behavior as "check the version in <file>" instead of
asserting a default. Agents: this file is a template, not a platform. -->

[One or two lines: which versions/config files decide the behavior described
here. If it wraps a host platform, say: "Also read `android.md` and `ios.md`
for host details."]

## Detection

- [Files and dependency declarations that identify the technology.]
- [How to tell its variants apart, and which host folders it contains.]

## What to inspect (Stage 1)

- **Architecture:** [ ]
- **UI and navigation:** [ ]
- **State:** [ ]
- **DI:** [ ]
- **Data:** [networking, serialization, persistence, secure storage]
- **Platform code:** [how it reaches native APIs]
- **Config:** [environments, build variants, lint rules]

## Mobile specifics

- **Lifecycle:** [ ]
- **State retention / process death:** [ ]
- **Back:** [ ]
- **Safe areas and keyboard:** [ ]
- **Accessibility:** [ ]
- **Theming / dark mode:** [ ]

## Tests and commands

- [Unit, UI and E2E test frameworks; which need a device.]
- Typical commands (verify against the project): [ ]

## Pitfalls to flag

- [ ]
