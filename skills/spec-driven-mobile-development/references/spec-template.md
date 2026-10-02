# SPEC: [feature name]

**Status:** Draft <!-- Draft | Draft — waiting for answers | In review | Approved (YYYY-MM-DD) -->
**Target platforms:** [from Stage 1, e.g. Android · iOS · Android + iOS]
**Source requirements:** [the user's original requirements, verbatim or linked (ticket, issue, document)]

<!-- FOR THE AGENT
- Write this document in the language of the project's existing feature docs,
  or the user's language if there are none. Translate these headings; keep the
  identifiers (FR, AC, D, UX — or the project's own scheme) unchanged. Keep
  these comments.
- Mark every statement as verified (cite path), confirmed (decision by the
  user), proposed, or PENDING. Never invent requirements or exclusions.
- No class, table, file or algorithm design here: that belongs to PLAN.md.
- Keep it proportional: delete sections marked (optional) that do not apply
  and list them under "Not applicable" with the reason. Prefer tables and short
  lines to prose. A reviewer should read it in about five minutes.
- Per-platform rows and columns use the target platforms above only.
- A complete document is not approved: present the gate and ask explicitly.
  Do not implement during this stage.
-->

## Goal and users

<!-- The need, who has it, and what they will be able to do. Product language. -->

[PENDING]

## Current situation

<!-- Relevant current behavior (with file paths), the limitation to solve, and
existing behavior that must be preserved. Include the mobile baseline facts
that matter for this feature. No architecture design. -->

[PENDING]

## Design references (optional)

<!-- Designs (Figma or other links), screenshots, design-system components to
use. Say which screens or states have no design yet. -->

- [PENDING]

## In scope

- **FR-01:** [PENDING]

**Out of scope (agreed with the user only; proposed exclusions go to Decisions):** [PENDING]

## User flows

<!-- Entry point, steps, outcome. Affected screens, navigation, alternatives. -->

1. [PENDING]

## Business rules and data (optional)

<!-- Required fields, validations, limits, ordering, duplicates, calculations,
roles and permissions. Meaning and behavior, not storage design. -->

- [PENDING]

## Error cases (optional)

<!-- Each case the user must be able to tell apart, and what they see and can
do. Do not map messages 1:1 to HTTP status codes. -->

| Case | Cause | What the user sees / can do |
| --- | --- | --- |
| [PENDING] | | |

## Service contract (optional — only if the feature uses a network)

<!-- Endpoints, request and response fields, error shapes, pagination, auth.
Mark each item as verified (source) or assumed. -->

[PENDING]

## Mobile behavior

<!-- Only the rows of the "Mobile behavior table" in mobile-guidelines.md (and
the project's guidelines) that this feature touches. Rows that do not apply
are not copied: list them in one line below with the reason. Observable
results, not mechanisms. Do not presume offline support or state retention. -->

| Situation | Expected behavior |
| --- | --- |
| [PENDING] | |

**Rows and guideline points not applicable:** [list · reason]

## Analytics, privacy and store declarations (optional)

<!-- Only if the feature emits analytics, collects or shares new data, adds an
SDK, or adds a permission. Events and properties (no personal data unless the
privacy policy allows it); feature flag name and default; which store
declarations must change: Google Play Data safety form and permission
declarations; App Store privacy labels and privacy manifest. -->

[PENDING]

## UI/UX suggestions (optional)

<!-- Improvements noticed while analysing. Each is a proposal until the user
confirms it; confirmed ones become an FR. -->

| # | Suggestion | Rationale | Status |
| --- | --- | --- | --- |
| UX-01 | [PENDING] | | Proposed |

## Constraints

<!-- Conditions already imposed by the user or the project rules: platforms,
minimum OS, dependency policy, architecture boundaries, measurable targets.
Not the agent's preferences. -->

- [PENDING]

## Acceptance criteria and how they are checked

<!-- Observable results; never "works correctly". Include agreed alternative
cases. Level: Unit (business/state logic) | Widget/UI | Device. Platform: target
platform(s), or Shared for logic tests run on the development machine. Tools
and commands go in PLAN.md. Do not mark criteria as passed here. -->

| Criterion | Given / when / then | Level | Platform |
| --- | --- | --- | --- |
| AC-01 · FR-01 | Given [context], when [action], then [observable result]. | [PENDING] | [PENDING] |

## Decisions

<!-- Confirmed decisions keep their ID and date. Pending ones list options and
the recommendation. Write "None pending" when all are resolved. -->

| ID | Question | Options | Recommendation | Status |
| --- | --- | --- | --- | --- |
| D-01 | [PENDING] | | | Pending |

## Not applicable

<!-- Sections removed and why, in one or two lines. -->

[e.g. Service contract — no network · Analytics — the project has none]

<!-- GATE CHECKLIST
Scope and exclusions agreed; flows consistent; every FR has at least one AC;
every AC has a level and platform; applicable guideline points covered, the
rest listed as not applicable; store declarations identified if data or
permissions change; no PENDING left unless the user deferred it explicitly.
-->
