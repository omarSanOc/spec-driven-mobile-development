---
name: spec-driven-mobile-development
description: Spec-Driven Mobile Development (SDMD) for iOS, Android, Kotlin Multiplatform, Flutter and React Native apps. Use when the user hands over new requirements, user stories, a ticket or a feature request, in any language (e.g. new requirements, feature spec, user story, nuevos requerimientos, nueva funcionalidad, historia de usuario), and wants them turned into an approved SPEC, PLAN and TASKS before any code is written, then implemented and validated with per-platform evidence. Also use to resume a feature that already has SDMD documents. Not for one-line fixes or general questions.
license: Apache-2.0
compatibility: Works with any agent that can read the repository. Building and testing need each platform's toolchain (Xcode on macOS for iOS); checks that cannot run are reported as BLOCKED.
metadata:
  author: Omar Sánchez
  version: "1.2.0"
  repository: https://github.com/omarSanOc/spec-driven-mobile-development
---

# Spec-Driven Mobile Development (SDMD)

The user gives you **only the new requirements**. Everything else — architecture,
conventions, business flow, tests, platform differences — you discover from the
repository. You then take the requirements through these stages:

```
0 Intake → 1 Context → 2 SPEC ✔ → 3 PLAN ✔ → 4 TASKS ✔ → 5 Implementation → 6 Validation
                       (approval)  (approval)  (approval +                     (report; sign-off
                                   └─ or one combined ─┘  authorization)        only if items open)
```

This file is tool-agnostic. "Ask the user" means whatever mechanism your
environment offers. If you cannot run commands, say which checks you could not
run instead of pretending.

| File | Read it |
| --- | --- |
| `references/context-discovery.md` | At the start of Stage 1 |
| `references/platforms/<platform>.md` | Stage 1, after detecting the platform(s) |
| `references/mobile-guidelines.md` | Stage 2 onward; keep it open until validation |
| `references/spec-template.md` · `plan-template.md` · `tasks-template.md` | Stages 2, 3, 4, if the project has no template of its own |
| `references/lite-template.md` | Lite track only |
| `references/project-context-template.md` | When saving reusable project context |
| `references/evidence-rules.md` | Stages 3, 5 and 6 |

---

## Non-negotiable rules

1. **No production code before authorization.** Stages 0–4 only create or edit
   feature documents. Implementation starts only when the user explicitly
   authorizes it. Approving a document is not authorizing implementation,
   unless the same message says so explicitly ("approved, go ahead and implement").
2. **Never invent.** Every statement is **Verified** (you read it; cite the
   path), **Confirmed** (the user decided it; record it as a decision),
   **Proposed** (your suggestion) or **PENDING**. No invented requirements,
   exclusions, files, APIs, versions, commands, test results or capabilities.
3. **Propose, recommend, wait.** A proposal becomes a decision only when the
   user confirms it. A product question is never answered by a technical assumption.
4. **Few questions at a time.** At most three per turn, highest impact first,
   each with 2–4 options, one marked as recommended with the reason.
5. **Precedence.** User's explicit instructions > the project's own rules
   (`AGENTS.md`, `CLAUDE.md`, guidelines, templates, a spec-driven tool already
   in use) > this skill's defaults. If project documents contradict each other
   in a way that changes behavior or scope, ask.
6. **Guidelines do not widen scope.** Mobile guidelines say what to *consider*;
   only what the feature touches becomes a requirement, and only once agreed.
7. **Evidence per platform.** Results are recorded for each **target platform**.
   Working on one platform proves nothing about another. A check that cannot
   run is BLOCKED, never passed. See `references/evidence-rules.md`.
8. **Language.** Talk in the user's language. Write documents in the language
   of the project's existing feature docs, else the user's. Code and comments
   follow the repository's convention.

**Identifiers:** `FR-01` requirement, `AC-01` acceptance criterion, `D-01`
decision, `UX-01` suggestion, `T-01` task — or the project's existing scheme
(e.g. `RF`/`CA`, Spec Kit or Kiro numbering), used consistently.

**Target platforms:** the platforms the app ships on, recorded in Stage 1.
Every per-platform row uses only that list; parity questions exist only with
more than one target.

---

## Tracks and approval modes

At the end of Stage 0, propose a track and an approval mode in one question.

**Track**
- **Full** (default): SPEC, PLAN and TASKS as separate documents.
- **Lite**: one `FEATURE.md` (`references/lite-template.md`). Only when all
  hold: one flow or screen; no new dependency; no new network call or contract
  change; no persistence or schema change; no new permission; about five tasks
  or fewer. If one stops holding, switch to full and carry over what was approved.

**Approval mode** (full track)
- **Step by step** (default): approve SPEC, then PLAN, then TASKS, then authorize.
- **Combined**: approve the SPEC; then you write PLAN and TASKS in one go and
  the user approves both together (and may authorize in the same reply). Use
  it when the user asks ("I trust you, go straight to TASKS") or accepts your
  suggestion for a feature with no open technical decisions.

The SPEC gate is never skipped or merged: it is where product decisions are
made. In combined mode you still stop mid-way if planning raises a question
that changes behavior, scope, dependencies or architecture. The user can
change mode at any time.

### How to present a gate

So that approvals are read, not rubber-stamped, every approval request comes
with a **review summary** of at most six bullets: decisions taken since the
last gate, proposals the user has not confirmed, risks, what was marked not
applicable, anything deferred, and where to look in the document. Ask for
approval only after the gate checklist of that stage passes.

---

## Resuming and unattended runs

**Resuming.** If the requirements belong to a feature that already has
documents, read them and their **Status** fields, resume at the first stage
that is not approved (or the first unticked task once implementation was
authorized), re-check the project context, and report where you are. If the
code changed in a way that affects approved documents, treat it as a change
after approval. Never repeat approved stages or re-ask recorded decisions.

**Nobody there to approve** (e.g. triggered from an issue or CI): do Stages
0–2 only, put every open question in the SPEC's "Decisions" table with options
and a recommendation, set the status to **Draft — waiting for answers**, and
stop. Silence is never approval or authorization.

---

## Stage 0 — Intake

1. If requirements arrive as links (ticket, issue, design), read them with
   your tools; if you cannot, ask the user to paste the content.
2. Restate the requirements as a numbered list without adding anything.
3. Flag ambiguities, missing actors or triggers, contradictions, and
   solutions disguised as needs — without resolving them.
4. If there are several requirements, propose how to group them into features
   (one SPEC per coherent feature).
5. Propose track and approval mode. Ask grouping, track and mode together.
6. Hold anything the repository might answer until Stage 1 is done.

## Stage 1 — Context (read-only)

Follow `references/context-discovery.md`: reuse a saved project context if
there is one, read the project's instructions and existing spec material,
detect the platform(s) and target platforms, inspect architecture, data,
tests, verified commands and conventions, and write the mobile baseline with
file evidence. For a repository with no app yet, it becomes a decision stage.

End with a short context report (platforms, architecture, flows touched, test
situation, baseline facts that matter, risks). No file dumps. No approval needed.

## Stage 2 — SPEC (what and why, not how)

Read `references/mobile-guidelines.md`. Create the SPEC from the project
template or `references/spec-template.md`; status **Draft**.

- Fill what you can verify; mark the rest PENDING. The template lists the
  sections and what each must contain.
- **Keep it proportional.** Remove sections that do not apply and list them in
  one line under "Not applicable", with the reason. In the mobile behavior
  table, include only the rows the feature touches; list the rest in one line.
- Architecture *constraints* belong here ("no new dependencies", "follow the
  existing state pattern"); class, table and file design belong to the PLAN.
- **Question loop:** ask the 1–3 highest-impact questions (scope → business
  rules → errors → mobile behavior → acceptance criteria → UI/UX proposals);
  after each answer update the SPEC and say what changed in one or two lines.
- **Gate:** scope and exclusions agreed; every FR has an AC; every AC has a
  check method and platform(s); every applicable guideline point covered;
  store declarations identified if the feature collects data or adds
  permissions; no PENDING left unless explicitly deferred. Then present the
  gate and mark **Approved** (with date) only after an explicit yes.

## Stage 3 — PLAN (how)

Use the project's template or `references/plan-template.md`; read
`references/evidence-rules.md`. Reference verified paths only, label new ones
as proposed, reuse before creating, plan every target's side of
platform-specific code in the same change, map each mobile decision of the
SPEC to its mechanism, add no dependency without the user's decision, and plan
validation per AC and target platform — including a **release-build check**
when evidence-rules requires it. Reference FR/AC/D by ID instead of copying
the SPEC. A behavior or scope question goes back to the SPEC.

## Stage 4 — TASKS

Use the project's template or `references/tasks-template.md`: small, ordered,
verifiable tasks `T-01…`, each with scope, limits, dependencies, the FR/AC it
serves and its validation; every task leaves every target compiling. List
release items outside the repository (store forms, console settings) as
separate items. End with the validation table. Present the gate (PLAN + TASKS
together in combined mode), then ask for authorization unless the user already
gave it explicitly.

## Stage 5 — Implementation (after explicit authorization)

- Execute tasks in order within the approved scope and architecture; after
  each, run its validation and record the observed result; tick only what was
  executed and passed.
- Follow existing conventions; no unrelated refactors, upgrades or
  opportunistic fixes. Write tests as planned; never weaken or delete a test
  to make it pass.
- A requirement gap or contradiction: stop that part, explain, propose
  options, update the documents after the user decides, continue.
- **Git:** no branches, commits, pushes or PRs unless the user or project rules
  ask. When asked, follow the project's conventions, else one commit per task
  referencing its ID and a PR description built from the SPEC goal, FRs, tasks
  and validation table.
- Do not ask again for steps already authorized.

## Stage 6 — Validation and close

- Fill the validation table for each AC and target platform: PASSED, FAILED,
  NOT RUN or BLOCKED (reason and the concrete check still needed).
- Run the full verification the project requires, plus the release-build
  check when it applies.
- Report what changed, documents updated, per-platform results, what remains
  open and why, and release items outside the repository.
- **Close:** if every row PASSED and no release item is pending, mark the
  feature **Done** — no sign-off question needed. Otherwise ask once whether to
  keep the feature open or close it with the open items recorded as follow-ups.
- If a saved project context exists and the feature changed a fact it records,
  propose the update.

---

## Every turn, start with a status line

```
SDMD · <feature> · <track>/<mode> · Stage <n> <name> · <document> <status>
```

Then what you did, what changed, and the next 1–3 questions. The document
holds the detail.

## Changes after approval

If a requirement changes after a stage was approved, update every affected FR,
AC, plan section and task, set those documents back to "In review", and present
the gate again before continuing.
