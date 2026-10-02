# Example · Forgot password (Flutter, Android + iOS)

> **Fictional project.** "Acme Notes", its paths, API and results are invented to
> show what SDMD produces. Nothing here describes a real codebase.

A complete run of the full track (step-by-step mode) for one small feature:

| File | Stage | What to look at |
| --- | --- | --- |
| [`PROJECT_CONTEXT.md`](PROJECT_CONTEXT.md) | 1 | Reusable context saved after the first feature; every fact cites a file |
| [`SPEC.md`](SPEC.md) | 2 | FR/AC traceability, error cases a user can tell apart, only the mobile rows that apply |
| [`PLAN.md`](PLAN.md) | 3 | Verified vs. proposed paths, mobile decision → mechanism, why no release-build check is needed |
| [`TASKS.md`](TASKS.md) | 4–6 | Small verifiable tasks, observed results, and a check reported as **BLOCKED** instead of passed |

The original request the user typed:

```
Use spec-driven-mobile-development with these requirements:
[Req 1] Users who forgot their password must be able to request a reset email from the login screen.
[Req 2] Show clear errors when something fails.
```

What the SPEC gate looked like (the review summary that comes with every approval request):

```
SDMD · forgot-password · full/step · Stage 2 SPEC · SPEC.md In review

Before you approve, check:
- Decided: 60 s resend cool-down (D-01), AcmeResultView for the confirmation (D-02), no exit confirmation (D-03).
- Confirmed from my proposals: pre-fill the email (UX-01 → FR-05). Rejected: open mail app.
- Privacy: the confirmation is identical for unknown emails; analytics carry no email.
- Accepted limitation: after process death the app reopens at login.
- Not applicable: empty/no-results states, offline, persistence, permissions.
Do you approve the SPEC?
```

This feature uses an existing endpoint for the first time, so it does not
qualify for the lite track (no new network calls). A UI-only change, such as
adding a "Show password" toggle, would.
