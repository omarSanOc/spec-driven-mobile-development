# PLAN: [feature name]

**SPEC:** [path to SPEC.md] · approved on [date]
**Target platforms:** [as in the SPEC]
**Status:** Draft <!-- Draft | In review | Approved (YYYY-MM-DD) -->

<!-- FOR THE AGENT
- Read the approved SPEC, the project instructions, the mobile guidelines, the
  platform reference(s) and evidence-rules.md. If the SPEC is not approved, say
  so and stop.
- Reference only verified paths; label new paths as proposed.
- Keep shared code shared; justify every descent into platform-specific code
  and plan every target platform's side in the same change.
- Reference FR/AC/D by ID instead of copying the SPEC.
- If a decision changes behavior or scope, go back to the SPEC and ask.
- Keep these comments. Do not implement during planning. Approval of this plan
  is not an authorization to implement.
-->

## Verified technical context

| Existing component | Verified path | Role in this feature |
| --- | --- | --- |
| [PENDING] | | |

**Reference implementation and conventions to follow:** [PENDING]

## Proposed solution

<!-- Approach and reasons. Responsibilities and the flow of data and events to
the UI. A diagram if it helps. -->

[PENDING]

## Affected modules and components

| Module / layer / source set | Exists / new | Changes | Dependencies affected |
| --- | --- | --- | --- |
| [PENDING] | | | |

| Component or path | Location | Action (reuse / modify / create) | Responsibility | FR |
| --- | --- | --- | --- | --- |
| [PENDING] | | | | |

**Platform-specific code:** <!-- expect/actual, platform channels, native
modules, per-target code: justification and every target's implementation.
"None" if everything is shared or the app has a single target. --> [PENDING]

## Data and contracts

- **Models and input/output contracts:** [PENDING]
- **Identifiers, relations, constraints:** [PENDING]
- **Data origin and transformations:** [PENDING]
- **Persistence, queries, updates:** [PENDING or N/A with reason]
- **Local vs remote data:** [PENDING or N/A with reason]
- **Compatibility and migrations:** [PENDING or N/A with reason]

## State, operations and errors

- **UI state and navigation:** [PENDING]
- **State retention and restoration:** [PENDING]
- **Execution, concurrency, cancellation:** [PENDING]
- **Errors, retries, duplicate prevention:** [PENDING]
- **Back handling on each target platform:** [PENDING]
- **Differences between target platforms:** [PENDING, None, or N/A — single target]
- **Analytics events and feature flag wiring:** [PENDING or N/A]
- **Other applicable mobile points and their mechanism:** [PENDING]

## Dependencies and configuration

<!-- None by default. Any addition: purpose, alternatives considered, proof it
supports every target, where it is declared. Requires the user's decision.
Permissions, manifest/Info.plist entries, privacy declarations, environments. -->

- [PENDING]

## Validation strategy

| Criterion | Method and test (existing / proposed) | Platform | Environment and data | Expected evidence |
| --- | --- | --- | --- | --- |
| AC-01 | [PENDING] | | | |

**Unit tests to add:** <!-- business rules, state transitions, each error case
named in the SPEC; fakes at the boundaries; where they live. --> [PENDING]

**Regression checks:** [PENDING]

**Verified build and test commands (one per affected target):** [PENDING]

**Device / emulator / simulator checks:** [PENDING]

**Environment limitations:** [what cannot be checked here and how it stays open]

## Implementation order

1. [Step, dependency, checkpoint]

## Risks and pending decisions

- **Risks and agreed mitigations:** [PENDING]
- **Pending decisions:** [PENDING or None]
