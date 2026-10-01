# Example · Forgot password (Flutter, Android + iOS)

> **Fictional project.** "Acme Notes", its paths, API and results are invented to
> show what SDMD produces. Nothing here describes a real codebase.

A complete run of the full track for one small feature:

| File | Stage | What to look at |
| --- | --- | --- |
| [`PROJECT_CONTEXT.md`](PROJECT_CONTEXT.md) | 1 | Reusable context saved after the first feature; every fact cites a file |
| [`SPEC.md`](SPEC.md) | 2 | FR/AC traceability, error cases a user can tell apart, the mobile behavior table, proposals vs. confirmed decisions |
| [`PLAN.md`](PLAN.md) | 3 | Verified vs. proposed paths, mobile decision → mechanism, one validation row per criterion and platform |
| [`TASKS.md`](TASKS.md) | 4–6 | Small verifiable tasks, observed results, and a check reported as **BLOCKED** instead of passed |

The original request the user typed:

```
Use spec-driven-mobile-development with these requirements:
[Req 1] Users who forgot their password must be able to request a reset email from the login screen.
[Req 2] Show clear errors when something fails.
```
