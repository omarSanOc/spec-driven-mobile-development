# TASKS: Forgot password

**SPEC:** [SPEC.md](SPEC.md) · approved on 2026-09-14
**PLAN:** [PLAN.md](PLAN.md) · approved on 2026-09-15
**Target platforms:** Android + iOS
**Implementation authorized:** 2026-09-15
**Execution status:** 5 of 5 tasks done · 7 of 8 criteria passed on every platform · 1 open item (AC-07 iOS)

## Summary

| | |
| --- | --- |
| Tasks | T-01 … T-05 |
| Requirements covered | FR-01 … FR-05 |
| Criteria to demonstrate | AC-01 … AC-08 |
| New dependencies | None |
| Inherited blockers | Physical iOS device for VoiceOver (AC-07) |

**Verification commands** (from `.github/workflows/ci.yml`):

```bash
analyze:     flutter analyze
test:        flutter test
build-android: flutter build apk --debug --dart-define-from-file=env/dev.json
build-ios:   flutter build ios --no-codesign --dart-define-from-file=env/dev.json
```

---

## Phase 1 · Data

### T-01 · Password reset request in the repository

- [x] **Objective:** `requestPasswordReset(email)` with failure mapping.
- **Scope:** `auth_repository.dart`, new `password_reset_failure.dart`, `test/features/auth/data/auth_repository_test.dart`.
- **Does not do:** UI, analytics.
- **Depends on:** none
- **Serves:** FR-03, FR-04, AC-04
- **Validation:** `test` — one test per case (202, no connection, 429 with and without `Retry-After`, 5xx, timeout).

**Result (2026-09-15):** 6 new tests, all passing. `analyze` clean.

## Phase 2 · State

### T-02 · ForgotPasswordCubit

- [x] **Objective:** states, validation, duplicate guard, cancellation, cool-down.
- **Scope:** `forgot_password_cubit.dart` + test.
- **Does not do:** UI.
- **Depends on:** T-01
- **Serves:** FR-02–FR-04, AC-02, AC-03, AC-04, AC-05, AC-08
- **Validation:** `test`.

**Result (2026-09-15):** 9 new tests passing; counting fake records 1 call for two submits (AC-05); fake clock 59 s → disabled, 60 s → enabled (AC-08).

## Phase 3 · UI

### T-03 · Page, route and login link

- [x] **Objective:** screen per design, `/forgot-password` route with pre-fill.
- **Scope:** `forgot_password_page.dart`, `router.dart`, `login_page.dart`, widget test.
- **Does not do:** analytics.
- **Depends on:** T-02
- **Serves:** FR-01, FR-05, AC-01, AC-06
- **Validation:** `analyze`, `test`, `build-android`, `build-ios`; manual AC-01 on both platforms.

**Result (2026-09-16):** widget test for pre-fill passing. Both builds succeed. AC-01 checked on Pixel 7 emulator (API 34) and iPhone 15 simulator (iOS 17).

### T-04 · Analytics event

- [x] **Objective:** `password_reset_requested` on success, no properties.
- **Scope:** cubit + test with a fake analytics sink.
- **Depends on:** T-02
- **Serves:** SPEC "Analytics"
- **Validation:** `test`.

**Result (2026-09-16):** test asserts one event with an empty property map.

## Phase 4 · Device checks

### T-05 · Manual checks on each platform

- [x] **Objective:** AC-03 end to end, large text, dark mode, back, AC-07.
- **Depends on:** T-03, T-04
- **Serves:** AC-03, AC-07, mobile behavior table
- **Validation:** steps in the validation table, on each platform.

**Result (2026-09-17):** all checks done except VoiceOver (no physical iOS device available; the simulator has no VoiceOver). Recorded as BLOCKED.

---

## Validation table

| Criterion | Evidence | Platform | Result | Notes |
| --- | --- | --- | --- | --- |
| AC-01 | Manual, emulator API 34 | Android | PASSED | |
| AC-01 | Manual, iPhone 15 simulator | iOS | PASSED | |
| AC-02 | `forgot_password_cubit_test.dart` | Shared | PASSED | 0 calls on the fake |
| AC-03 | Cubit test + manual against dev | Android | PASSED | |
| AC-03 | Cubit test + manual against dev | iOS | PASSED | |
| AC-04 | Repository + cubit tests, one per case | Shared | PASSED | 4 cases |
| AC-05 | Counting fake | Shared | PASSED | 1 call |
| AC-06 | Widget test | Shared | PASSED | |
| AC-07 | TalkBack on Pixel 6 (physical) | Android | PASSED | Error and confirmation announced |
| AC-07 | — | iOS | BLOCKED | No physical iOS device; simulator has no VoiceOver |
| AC-08 | Cubit test with fake clock | Shared | PASSED | |

## Release items outside the repository

None (store declarations unchanged).

## Open items

- **AC-07 · iOS:** on a physical iPhone with VoiceOver on, submit an invalid
  email and a valid one; confirm the error and the confirmation are announced.
