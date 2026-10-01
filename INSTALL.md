# Installing `spec-driven-mobile-development`

The skill is the folder `skills/spec-driven-mobile-development/` (a `SKILL.md`
plus `references/`). Install it **per project** (shared with your team through
git) or **personally** (available in all your projects).

## Option 1 · `skills` CLI (any agent)

```bash
npx skills add omarSanOc/spec-driven-mobile-development
```

The CLI asks which agents to install for and puts the folder where each one
looks for skills.

## Option 2 · Claude Code plugin

```
/plugin marketplace add omarSanOc/spec-driven-mobile-development
/plugin install spec-driven-mobile-development@sdmd
```

Update later with `/plugin marketplace update sdmd`.

## Option 3 · Copy the folder

Copy `skills/spec-driven-mobile-development/` to one of these locations, keeping
the folder name:

| Agent | Project (commit it) | Personal |
| --- | --- | --- |
| Claude Code | `.claude/skills/` | `~/.claude/skills/` |
| OpenAI Codex | `.agents/skills/` | `~/.agents/skills/` |
| Gemini CLI | `.agents/skills/` or `.gemini/skills/` | `~/.agents/skills/` or `~/.gemini/skills/` |
| VS Code / GitHub Copilot | `.agents/skills/`, `.github/skills/` or `.claude/skills/` | `~/.agents/skills/`, `~/.copilot/skills/` or `~/.claude/skills/` |
| Other Agent Skills–compatible agents | check the agent's docs; most read `.agents/skills/` | |

To serve several agents from one copy, keep it in `.agents/skills/` and, for
Claude Code, add a symlink:

```bash
mkdir -p .claude
ln -s ../.agents/skills .claude/skills
```

On Windows, where symlinks may need extra permissions, copy the folder to both
locations instead.

Agent paths change over time; if a skill does not show up, check the agent's
current documentation.

## Agents without skill support

Copy the folder anywhere in the repo (e.g. `.agents/skills/`) and add this to
`AGENTS.md`, or to the agent's instruction file (`.cursor/rules/`,
`.github/copilot-instructions.md`, `.windsurfrules`, …):

> For new requirements, follow `.agents/skills/spec-driven-mobile-development/SKILL.md`
> and read its `references/` files when the workflow says so.

## Claude apps (claude.ai, desktop)

Zip the `spec-driven-mobile-development/` folder and upload it as a custom
skill in the app's settings. See Anthropic's documentation for the current
location of the option.

## Check that it works

Ask your agent: *"Use spec-driven-mobile-development with these requirements:
[Req 1] …"*. The first reply should restate your requirements, flag
ambiguities, propose a track, and start with a status line beginning `SDMD ·`.

## Project rules

If the repo already has `AGENTS.md`, `MOBILE_GUIDELINES.md`, its own SPEC/PLAN/
TASKS templates, or uses Spec Kit, Kiro or OpenSpec, the skill uses them and
they take priority over its defaults.
