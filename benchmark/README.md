# Before / after: measuring SDMD on a real feature

This folder holds the **protocol** for comparing the same real feature built
with and without the skill. No results are published here until a real run
has been done; please do not add estimated or simulated numbers.

## What is being tested

Whether an agent using SDMD ships a mobile feature with fewer defects and less
rework than the same agent given the same requirements without it — and what
it costs in time, tokens and approvals.

## Setup

1. **Pick a real feature** from a real repository, small to medium (one to
   three screens), that touches at least one of: network errors, a form,
   persistence, permissions, analytics. Write the requirements as the user
   would, in two to five lines.
2. **Freeze the starting point:** note the commit hash. Create two branches
   (or worktrees) from it: `bench/without-sdmd` and `bench/with-sdmd`.
3. **Same agent, same model, same settings** for both runs. Record them.
4. **Write the product answers first.** Before any run, write down the answers
   to the product questions you expect (cool-downs, messages, what happens
   offline…). During both runs, answer **only what the agent asks**, using that
   sheet. Do not volunteer information in the run without the skill that you
   would not give in the run with it.
5. **Run A — without SDMD:** remove or disable the skill; prompt
   "Implement these requirements: …". Let it finish.
6. **Run B — with SDMD:** prompt "Use spec-driven-mobile-development with these
   requirements: …". Approve gates after reading them (note how long each
   took), authorize, let it finish.
7. **Do not fix anything by hand** in either branch before the evaluation.

## Evaluation (same for both branches)

Run the **QA script** below on every target platform, on a release build, and
record each item as OK / Defect / Not handled. Prepare the script from the
requirements before the runs so it does not favour either branch.

| # | Scenario | Android | iOS |
| --- | --- | --- | --- |
| 1 | Happy path | | |
| 2 | Each error case the requirements imply (no connection, server error, invalid input…) | | |
| 3 | Double tap on the primary action | | |
| 4 | Back / swipe back mid-operation and with a half-filled form | | |
| 5 | Background and return; Android rotation or font-size change | | |
| 6 | Kill the process and reopen | | |
| 7 | Largest text size; dark mode | | |
| 8 | TalkBack / VoiceOver | | |
| 9 | Permission denied (if any) | | |
| 10 | Release build (minified) works through the whole flow | | |

Then record:

| Metric | Without SDMD | With SDMD |
| --- | --- | --- |
| Defects found by the QA script (count, per platform) | | |
| Scenarios not handled at all | | |
| Unit tests added · error cases covered by a test | | |
| Files changed outside the feature's scope | | |
| "Done" claims that were false (said it works, it did not) | | |
| Fix-up commits needed after QA to reach "shippable" | | |
| Questions the agent asked you | | |
| Your time: answering + reviewing + fixing (minutes) | | |
| Tokens / cost, if your tool reports it | | |
| Wall-clock time to "shippable" | | |

## Reporting

Copy [`results-template.md`](results-template.md) to
`results/<date>-<feature>.md`, fill it, and include the two branch links or
the diffs. Report what favoured the run without the skill too (it is usually
faster to a first build). One run is an anecdote; say so.
