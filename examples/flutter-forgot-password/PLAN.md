# PLAN: Forgot password

**SPEC:** [SPEC.md](SPEC.md) · approved on 2026-09-14
**Target platforms:** Android + iOS
**Status:** Approved (2026-09-15)

## Verified technical context

| Existing component | Verified path | Role in this feature |
| --- | --- | --- |
| Login screen | `lib/features/auth/presentation/login_page.dart` | Entry point (link + email to pre-fill) |
| Auth repository | `lib/features/auth/data/auth_repository.dart` | Gets the new request method |
| API client | `lib/core/network/api_client.dart` | Transport, timeouts, `CancelToken` |
| Email validator | `lib/features/auth/domain/email_validator.dart` | Reused as is |
| Router | `lib/app/router.dart` | New route |
| Result view | `lib/design_system/acme_result_view.dart` | Confirmation (D-02) |
| Analytics | `lib/core/analytics/analytics.dart` | Event on success |

**Reference implementation and conventions to follow:** `LoginCubit` and its tests in `test/features/auth/`.

## Proposed solution

`ForgotPasswordPage` → `ForgotPasswordCubit` → `AuthRepository.requestPasswordReset(email)`
→ `ApiClient`. The repository maps transport results to a sealed
`PasswordResetFailure` (`noConnection`, `tooManyRequests(retryAfter)`, `server`).
The cubit exposes `editing → submitting → success(email) | failure(type)`.

## Affected modules and components

| Component or path | Location | Action | Responsibility | FR |
| --- | --- | --- | --- | --- |
| `requestPasswordReset` | `auth_repository.dart` | modify | Call + error mapping | FR-03, FR-04 |
| `PasswordResetFailure` | `lib/features/auth/domain/password_reset_failure.dart` *(proposed)* | create | Failure types | FR-04 |
| `ForgotPasswordCubit` | `lib/features/auth/presentation/forgot_password_cubit.dart` *(proposed)* | create | State, duplicate guard, cancel, resend cool-down | FR-02–FR-04 |
| `ForgotPasswordPage` | `lib/features/auth/presentation/forgot_password_page.dart` *(proposed)* | create | UI, pre-fill | FR-01–FR-05 |
| Route `/forgot-password` | `router.dart` | modify | Navigation with `extra: email` | FR-01, FR-05 |
| Login link | `login_page.dart` | modify | Navigate | FR-01 |

**Platform-specific code:** None.

## Data and contracts

- **Models and contracts:** request body is a plain map `{email}`; responses per SPEC "Service contract". Nothing is stored.

## State, operations and errors

- **UI state and navigation:** cubit states above; `go_router` push/pop.
- **State retention:** cubit lives in the route; survives Android recreation; process death → login (SPEC).
- **Cancellation:** `CancelToken` cancelled in `close()` when the route is popped.
- **Duplicate prevention:** `submit()` ignored while `submitting`.
- **Errors and retries:** no automatic retry; 15 s timeout from `ApiClient`.
- **Resend cool-down:** the success state carries `resendAvailableAt`; the cubit uses an injected `Clock` so tests control time.
- **Back handling:** default pop on both platforms; no `PopScope` (D-03).
- **Differences between target platforms:** None.
- **Analytics:** `Analytics.track('password_reset_requested')` in the cubit on success.

## Dependencies and configuration

- None. **Store declarations:** no change (SPEC).

## Validation strategy

| Criterion | Method and test | Platform | Environment | Expected evidence |
| --- | --- | --- | --- | --- |
| AC-01 | Manual | Android, iOS | Emulator API 34 · iPhone 15 simulator | Screen opens |
| AC-02 | `forgot_password_cubit_test.dart` *(proposed)* | Shared | `flutter test` | Test passes; fake records 0 calls |
| AC-03 | Cubit test + manual | Shared + Android, iOS | dev server | Test passes; confirmation seen |
| AC-04 | `auth_repository_test.dart` + cubit test, one per case | Shared | Fake `HttpClientAdapter` | 4 tests pass |
| AC-05 | Cubit test with counting fake | Shared | `flutter test` | 1 call |
| AC-06 | Widget test | Shared | `flutter test` | Field contains email |
| AC-07 | Manual with TalkBack / VoiceOver | Android, iOS | Physical devices | Announcements heard |
| AC-08 | Cubit test with an injected clock | Shared | `flutter test` | Disabled at 59 s, enabled at 60 s |

**Verified commands:** see `PROJECT_CONTEXT.md` → Commands.

**Release-build check:** not required — no new library, no new serialized model (the body is a map, errors are mapped by hand), no build configuration change.

**Environment limitations:** VoiceOver is not available in the iOS simulator; AC-07 on iOS needs a physical device.

## Implementation order

1. Failure type + repository method + tests → `flutter test`.
2. Cubit + tests → `flutter test`.
3. Page, route, login link → `flutter analyze`, both builds.
4. Analytics event → `flutter test`.
5. Widget test + manual checks on both platforms.

## Risks and pending decisions

- **Risk:** `Retry-After` missing on some 429s → fall back to "Try again later". *(Agreed)*
- **Pending decisions:** None.
