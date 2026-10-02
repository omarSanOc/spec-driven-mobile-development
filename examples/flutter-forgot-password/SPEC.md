# SPEC: Forgot password

**Status:** Approved (2026-09-14)
**Target platforms:** Android + iOS
**Source requirements:** "[Req 1] Users who forgot their password must be able to request a reset email from the login screen. [Req 2] Show clear errors when something fails."

## Goal and users

Signed-out users who forgot their password can ask for a reset email from the
login screen, without contacting support, and know whether it went through. *(Confirmed)*

## Current situation

- "Forgot password?" on login does nothing: `onPressed: null` in `lib/features/auth/presentation/login_page.dart`. *(Verified)*
- The API already exposes `POST /v1/auth/password-reset` — `docs/api/openapi.yaml`. *(Verified)*
- Baseline that matters: no state restoration, portrait only, dark mode follows the system (see `PROJECT_CONTEXT.md`).
- Must be preserved: the login flow and its error handling.

## Design references

- Figma *Auth › Forgot password* (request screen). *(Provided by the user)*
- No design for the confirmation → `AcmeResultView` from the design system (D-02). *(Confirmed)*

## In scope

- **FR-01:** From login, the user opens a "Forgot password" screen.
- **FR-02:** The user enters an email; the format is validated before sending.
- **FR-03:** After sending, a confirmation shows the email, "Back to sign in" and "Resend". It is the same whether or not an account exists.
- **FR-04:** Each failure shows its own message (see "Error cases") and lets the user retry.
- **FR-05:** The email typed on login is pre-filled. *(From UX-01, confirmed)*

**Out of scope (agreed):** setting the new password (web link in the email); opening the mail app (UX-02, rejected); deep links back into the app.

## User flows

1. Login → "Forgot password?" → screen (email pre-filled if typed) → "Send link" → loading → confirmation → "Back to sign in".
2. Invalid email → inline error, nothing sent. Failure → case message → retry.

## Business rules and data

- Email trimmed, max 254 characters, same validator as login (`lib/features/auth/domain/email_validator.dart`). *(Verified)*
- "Resend" stays disabled for 60 s after a success, with a visible countdown (D-01). *(Confirmed)*

## Error cases

| Case | Cause | What the user sees / can do |
| --- | --- | --- |
| Invalid email | Format check fails | Inline field error |
| No connection | Request cannot reach the server | "No internet connection. Check it and try again." + Retry |
| Too many requests | 429 | "Too many attempts. Try again in N minutes." (N from `Retry-After`; "later" if missing) |
| Server problem | 5xx or 15 s timeout | "Something went wrong on our side. Try again." + Retry |

## Service contract

`POST /v1/auth/password-reset` · `{ "email": string }` · `202` also for unknown
emails · `429` with `Retry-After` · `5xx`. *(Verified in `docs/api/openapi.yaml`)*

## Mobile behavior

| Situation | Expected behavior |
| --- | --- |
| Action in progress | Button shows progress and is disabled; field read-only |
| Invalid input | Inline error after "Send link"; cleared on edit |
| Excessive wait / no connection | 15 s limit → "Server problem"; no connection → its case; no automatic retry |
| Repeated taps on "Send link" | Only one request is sent |
| Back (Android system back, iOS edge swipe) | Returns to login without confirmation (D-03); an in-flight request is cancelled |
| Background and return · *Android* recreation | Typed email and current state remain |
| Reopen after process death | Opens at login (baseline: no restoration). *(Confirmed acceptable)* |
| Large text, screen reader, dark mode | No clipping at the largest sizes; errors and confirmation announced; theme tokens only |
| Differences between Android and iOS | None |

**Rows not applicable:** empty state and no results (form, no list).

## Analytics, privacy and store declarations

- Event `password_reset_requested` on a 202, no properties (the email is personal data). *(Confirmed)*
- App interactions are already declared in Play Data safety and App Store privacy labels; no change. *(Confirmed by the user)*

## UI/UX suggestions

| # | Suggestion | Rationale | Status |
| --- | --- | --- | --- |
| UX-01 | Pre-fill the email from login | Saves typing | Confirmed → FR-05 |
| UX-02 | "Open mail app" button | Shortcut to the email | Rejected |

## Constraints

- No new dependencies (`CONTRIBUTING.md`); follow the Cubit pattern of `lib/features/auth/`. *(Confirmed)*

## Acceptance criteria and how they are checked

| Criterion | Given / when / then | Level | Platform |
| --- | --- | --- | --- |
| AC-01 · FR-01 | On login, tapping "Forgot password?" opens the screen | UI | Android, iOS |
| AC-02 · FR-02 | Invalid email + "Send link" → inline error, no request | Unit | Shared |
| AC-03 · FR-03 | Valid email + 202 → confirmation with the email and both actions | Unit + UI | Shared, Android, iOS |
| AC-04 · FR-04 | Each failure case → its message and Retry | Unit | Shared |
| AC-05 · FR-03 | Second tap while sending → no second request | Unit | Shared |
| AC-06 · FR-05 | Email typed on login → field pre-filled | Widget | Shared |
| AC-07 · FR-04 | Screen reader on → error and confirmation announced | Device | Android, iOS |
| AC-08 · FR-03 | Before 60 s "Resend" is disabled with the countdown; after, enabled | Unit | Shared |

## Decisions

| ID | Question | Options | Recommendation | Status |
| --- | --- | --- | --- | --- |
| D-01 | Cool-down before resending? | None · 30 s · 60 s | 60 s (server rate limit) | Confirmed 2026-09-13 |
| D-02 | Confirmation design? | Wait for Figma · `AcmeResultView` | `AcmeResultView` | Confirmed 2026-09-13 |
| D-03 | Confirm before leaving with a typed email? | Yes · No | No (one field) | Confirmed 2026-09-13 |

## Not applicable

Permissions, background work, offline-first and persistence: nothing is stored or requested. No feature flag.
