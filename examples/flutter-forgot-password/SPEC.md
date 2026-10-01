# SPEC: Forgot password

**Status:** Approved (2026-09-14)
**Target platforms:** Android + iOS
**Source requirements:** "[Req 1] Users who forgot their password must be able to request a reset email from the login screen. [Req 2] Show clear errors when something fails."

## Goal and users

Signed-out users who forgot their password can ask for a reset email from the
login screen, without contacting support, and know whether the request went
through. *(Confirmed)*

## Current situation

- The login screen shows a "Forgot password?" link that does nothing:
  `onPressed: null` in `lib/features/auth/presentation/login_page.dart`. *(Verified)*
- The API already exposes `POST /v1/auth/password-reset` —
  `docs/api/openapi.yaml`. *(Verified)*
- Baseline that matters: no state restoration, portrait only, dark mode follows
  the system, analytics must not carry personal data (see `PROJECT_CONTEXT.md`).
- Must be preserved: the login flow and its error handling.

## Design references

- Figma: *Auth › Forgot password* (request screen). *(Provided by the user)*
- No design for the confirmation state → use `AcmeResultView` from the design
  system (D-02). *(Confirmed)*

## In scope

- **FR-01:** From the login screen, the user opens a "Forgot password" screen.
- **FR-02:** The user enters an email; the format is validated before sending.
- **FR-03:** The user sends the request and sees a confirmation that shows the
  email, a "Back to sign in" action and a "Resend" action. The confirmation is
  the same whether or not an account exists for that email.
- **FR-04:** Failures show a distinct message per case (see "Error cases") and
  let the user try again.
- **FR-05:** If the login email field has a value, the new screen opens with it
  pre-filled. *(From UX-01, confirmed)*

## Out of scope

- Setting the new password (handled by the web link in the email). *(Confirmed)*
- Opening the mail app from the confirmation (UX-02, rejected). *(Confirmed)*
- Deep links back into the app. *(Confirmed)*

## User flows

1. Login → "Forgot password?" → Forgot password screen (email pre-filled if typed).
2. User edits the email → "Send link" → loading → confirmation → "Back to sign in" returns to login.
3. Alternative: invalid email → inline error, nothing is sent.
4. Alternative: failure → message for the case → user can retry.

## Business rules and data

- Email is trimmed; max 254 characters; format checked with the same validator
  as login (`lib/features/auth/domain/email_validator.dart`). *(Verified)*
- After a successful request, "Resend" stays disabled for 60 s, with a
  visible countdown (D-01). *(Confirmed)*

## Error cases

| Case | Cause | What the user sees / can do |
| --- | --- | --- |
| Invalid email | Format check fails | Inline field error; can correct and send |
| No connection | Request cannot reach the server | "No internet connection. Check it and try again." + Retry |
| Too many requests | Server answers 429 | "Too many attempts. Try again in N minutes." (N from `Retry-After`) |
| Server problem | 5xx or timeout (15 s) | "Something went wrong on our side. Try again." + Retry |

## Service contract

`POST /v1/auth/password-reset` · body `{ "email": string }` · `202` on accept
(also for unknown emails) · `429` with `Retry-After` seconds · `5xx` generic.
*(Verified in `docs/api/openapi.yaml`)*

## Mobile behavior and alternative cases

| Situation | Expected behavior |
| --- | --- |
| Loading or action in progress | Button shows progress and is disabled; field read-only |
| No data (first use / empty) | N/A — form screen with no list |
| No results for the current filters | N/A — no filters |
| Invalid input | Inline error under the field after "Send link"; cleared when the user edits |
| Error or excessive wait | 15 s limit, then the "Server problem" case |
| No connection or connection lost mid-operation | "No connection" case; nothing is retried automatically |
| Repeated taps on the primary action | Only one request is sent |
| Cancel or go back — *Android:* system back / predictive back | Returns to login without confirmation; an in-flight request is cancelled |
| Cancel or go back — *iOS:* edge swipe | Same as Android |
| Background and return | Screen and typed email remain; an in-flight request continues |
| *Android:* screen recreation (configuration change) | Typed email and current state remain |
| Reopen after process death | App opens at login (baseline: no restoration). *(Confirmed acceptable)* |
| Large text / accessibility services on | No clipping at 200 % / largest Dynamic Type; screen reader announces errors and the confirmation |
| Dark mode / appearance change | Uses theme tokens; readable in both |
| Differences between target platforms | None |

**Guideline points not applicable, and why:** permissions, background work,
offline-first (no data to cache), persistence (nothing stored).

## Analytics and feature flags

- Event `password_reset_requested` on a 202, with no properties (the email is
  personal data). *(Confirmed)*
- No feature flag. *(Confirmed)*

## UI/UX suggestions (proposals)

| # | Suggestion | Rationale | Status |
| --- | --- | --- | --- |
| UX-01 | Pre-fill the email from the login field | Saves typing; common pattern | Confirmed → FR-05 |
| UX-02 | "Open mail app" button on the confirmation | Shortcut to the email | Rejected (out of scope) |

## Constraints

- No new dependencies. *(Project rule, `CONTRIBUTING.md`)*
- Follow the Cubit pattern of `lib/features/auth/`. *(Confirmed)*

## Acceptance criteria

- **AC-01 · FR-01:** Given the login screen, when the user taps "Forgot password?", then the Forgot password screen opens.
- **AC-02 · FR-02:** Given an invalid email, when the user taps "Send link", then an inline error appears and no request is sent.
- **AC-03 · FR-03:** Given a valid email, when the server answers 202, then the confirmation shows that email and "Back to sign in".
- **AC-04 · FR-04:** Given each failure in "Error cases", when it happens, then its specific message and a retry are shown.
- **AC-05 · FR-03:** Given a request in progress, when the user taps "Send link" again, then no second request is sent.
- **AC-06 · FR-05:** Given an email typed on login, when the screen opens, then the field contains it.
- **AC-07 · FR-04:** Given a screen reader is on, when an error or the confirmation appears, then it is announced.
- **AC-08 · FR-03:** Given the confirmation was just shown, when less than 60 s have passed, then "Resend" is disabled and shows the remaining seconds; after 60 s it is enabled.

## How each criterion is checked

| Criterion | Conditions and steps | Expected result | Level | Platform |
| --- | --- | --- | --- | --- |
| AC-01 | Tap the link on login | Screen opens | UI | Android, iOS |
| AC-02 | Submit `foo@` | Inline error, zero requests | Unit | Shared |
| AC-03 | Fake 202 | Success state with the email | Unit + UI | Shared + Android, iOS |
| AC-04 | Fake no-connection, 429, 5xx, timeout | One state per case | Unit | Shared |
| AC-05 | Two submits while loading | One call on the fake | Unit | Shared |
| AC-06 | Type email on login, open screen | Field pre-filled | Widget | Shared |
| AC-07 | TalkBack / VoiceOver on, trigger error and success | Both announced | Device | Android, iOS |
| AC-08 | Fake clock: 59 s, then 60 s | Disabled, then enabled | Unit | Shared |

## Decisions

| ID | Question | Options | Recommendation | Status |
| --- | --- | --- | --- | --- |
| D-01 | Cool-down before resending? | None · 30 s · 60 s | 60 s (matches server rate limit) | Confirmed 2026-09-13 |
| D-02 | Confirmation design? | Wait for Figma · `AcmeResultView` | `AcmeResultView` | Confirmed 2026-09-13 |
| D-03 | Confirm before leaving with a typed email? | Yes · No | No (one field, nothing lost) | Confirmed 2026-09-13 |
