# Contributing

Thanks for helping make SDMD useful for more mobile teams. Issues and pull
requests are welcome in English or Spanish.

## What helps most

- **A new platform file** (e.g. .NET MAUI, Capacitor, NativeScript, Compose
  Multiplatform for desktop): copy
  [`platforms/_template.md`](skills/spec-driven-mobile-development/references/platforms/_template.md),
  fill it, add its detection row to `SKILL.md` (Stage 1.3) in the right
  precedence position, and list it in both READMEs.
- **Corrections to version-dependent facts** (APIs, defaults, commands that
  changed). Link the official source in the PR.
- **Examples** in `examples/` for other technologies. Mark them as fictional
  and keep them short.
- **Before/after results** following [`benchmark/`](benchmark/README.md),
  from real runs only.
- **Reports from real use:** where the agent asked too much, too little, or got
  stuck. Include the agent and model, the stage, and the status line.

## Writing guidelines

- `SKILL.md` stays under 500 lines; detail goes into `references/`.
- Instructions are written for the agent: imperative, specific, and testable.
  Prefer "check X in file Y" over asserting a default that may change.
- Keep the skill tool-agnostic: no instructions that only one agent can follow.
- Platform-specific notes belong in `platforms/*.md`; generic rules mention a
  platform only as an example tagged *Android* or *iOS*.
- English in the skill files. Templates tell the agent to translate headings.

## Before opening a PR

- Every file path mentioned in `SKILL.md` exists.
- Relative links in the READMEs and examples resolve.
- `.claude-plugin/*.json` are valid JSON, and versions match `SKILL.md`
  metadata when you bump one.
- Add an entry to `CHANGELOG.md` under an "Unreleased" heading.

## License

By contributing you agree that your contributions are licensed under the
[Apache License 2.0](LICENSE).
