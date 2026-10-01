# TASKS: [feature name]

**SPEC:** [SPEC.md](SPEC.md) · approved on [date]
**PLAN:** [PLAN.md](PLAN.md) · approved on [date]
**Target platforms:** [as in the SPEC]
**Implementation authorized:** [date, or "No"]
**Execution status:** [n of N tasks done · n of M criteria passed · open items]

<!-- HOW TO USE THIS DOCUMENT
- A task is ticked only when its validation was executed and the result
  observed. An unexecuted command or an unrun test is not evidence.
- A check that cannot run (no macOS, Xcode, simulator, emulator or device) is
  recorded as BLOCKED with its reason, never as passed.
- Every task leaves the project compiling on every target.
- The validation table at the end closes the feature: until each row has an
  observed result, the feature is not finished.
-->

## Summary

| | |
| --- | --- |
| Tasks | T-01 … T-NN |
| Requirements covered | FR-01 … |
| Criteria to demonstrate | AC-01 … |
| New dependencies | None / list with the decision that authorized them |
| Inherited blockers | [decisions or environments still missing] |

**Verification commands** (from the project docs or CI; referenced by short name below):

```bash
# one build command per target platform · unit tests (per target) · lint
```

---

## Phase 1 · [name]

### T-01 · [short title]

- [ ] **Objective:** [what this task achieves]
- **Scope:** [files, components, configuration]
- **Does not do:** [explicit limits]
- **Depends on:** [T-xx or none]
- **Serves:** [FR-xx, AC-xx, D-xx]
- **Validation:** [commands and/or manual steps, and platform]

**Result (date):** [what was done and what was observed — filled during implementation]

---

## Validation table

<!-- One row per criterion and target platform. With a single target platform,
one row per criterion. -->

| Criterion | Evidence | Platform | Result | Notes |
| --- | --- | --- | --- | --- |
| AC-01 | | [target platform] | NOT RUN | |

<!-- Result: PASSED | FAILED | NOT RUN | BLOCKED (reason) -->

## Open items

- [Anything NOT RUN or BLOCKED, with the concrete check needed to close it]
