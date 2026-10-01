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
- Per-platform rows and columns use the target platforms above only.
- Keep it proportional to the feature. A complete document is not approved:
  ask for approval explicitly. Do not implement during this stage.
-->

## Goal and users

<!-- The need, who has it, and what they will be able to do. Product language. -->

[PENDING]

## Current situation

<!-- Relevant current behavior (with file paths), the limitation to solve, and
existing behavior that must be preserved. Include the mobile baseline facts
that matter for this feature. No architecture design. -->

[PENDING]

## Design references

<!-- Designs (Figma or other links), screenshots, design-system components to
use. Say which screens or states have no design yet. N/A if the feature has no UI. -->

- [PENDING]

## In scope

- **FR-01:** [PENDING]
- **FR-02:** [PENDING]

## Out of scope

<!-- Only exclusions agreed with the user. Proposed exclusions go under
"Decisions" until confirmed. -->

- [PENDING]

## User flows

<!-- Entry point, steps, outcome. Affected screens, navigation, alternatives. -->

1. [PENDING]

## Business rules and data

<!-- Required fields, validations, limits, ordering, duplicates, calculations,
roles and permissions. Meaning and behavior, not storage design. -->

- [PENDING]

## Error cases

<!-- Each case the user must be able to tell apart, and what they see and can
do. Do not map messages 1:1 to HTTP status codes. -->

| Case | Cause | What the user sees / can do |
| --- | --- | --- |
| [PENDING] | | |

## Service contract

<!-- Only if the feature uses a network. Endpoints, request and response fields,
error shapes, pagination, auth. Mark each item as verified (source) or assumed.
Write N/A with reason otherwise. -->

[N/A or PENDING]

## Mobile behavior and alternative cases

<!-- Copy the rows of the "Mobile behavior table" in mobile-guidelines.md (and
the project's guidelines). Omit rows tagged with a non-target platform; omit
the platform-differences row with a single target. Observable results, not
mechanisms. Do not presume offline support or full state retention. -->

| Situation | Expected behavior |
| --- | --- |
| Loading or action in progress | [PENDING] |
| … (remaining rows from mobile-guidelines.md) | [PENDING] |

**Guideline points not applicable, and why:** [PENDING]

## Analytics and feature flags

<!-- Only if the project uses them or the user asks. Event names, properties,
consent; flag name, default and behavior when off. N/A with reason otherwise. -->

[N/A or PENDING]

## UI/UX suggestions (proposals)

<!-- Improvements noticed while analysing: accessibility, empty/error states,
feedback, copy, consistency with the design system. Each one is a proposal
until the user confirms it; confirmed ones move to "In scope" as an FR. -->

| # | Suggestion | Rationale | Status |
| --- | --- | --- | --- |
| UX-01 | [PENDING] | | Proposed |

## Constraints

<!-- Conditions already imposed by the user or the project rules: platforms,
minimum OS, dependency policy, architecture boundaries, measurable performance
or accessibility targets, required technology. Not the agent's preferences. -->

- [PENDING]

## Acceptance criteria

<!-- Observable results. Never "works correctly". Include agreed alternative cases. -->

- **AC-01 · FR-01:** Given [context], when [action], then [observable result].
- **AC-02 · FR-02:** Given [context], when [action], then [observable result].

## How each criterion is checked

<!-- One row per criterion. Level: Unit (business/state logic) | UI | Device.
Platform: one or more of the target platforms. Tools and commands go in PLAN.md.
Do not mark criteria as passed during specification. -->

| Criterion | Conditions and steps | Expected result | Level | Platform |
| --- | --- | --- | --- | --- |
| AC-01 | [PENDING] | [PENDING] | [PENDING] | [PENDING] |

## Decisions

<!-- Confirmed decisions keep their ID and date. Pending ones list options and
the recommendation. Write "None pending" when all are resolved. -->

| ID | Question | Options | Recommendation | Status |
| --- | --- | --- | --- | --- |
| D-01 | [PENDING] | | | Pending |

<!-- BEFORE REQUESTING APPROVAL
Scope and exclusions agreed; flows consistent; every FR has at least one AC;
every AC has a check method and platform; applicable guideline points covered
or N/A with reason; no PENDING left unless the user deferred it explicitly.
-->
