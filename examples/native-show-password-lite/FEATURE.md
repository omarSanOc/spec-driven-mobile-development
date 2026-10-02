# FEATURE: Show password toggle

> **Fictional project.** A native app with separate Android (Jetpack Compose)
> and iOS (SwiftUI) codebases. Paths and results are invented to show the
> **lite track**: one document, two approvals.

**Track:** Lite · approval in one reply for Part 2 + authorization
**Target platforms:** Android + iOS
**Source requirements:** "Users want to see the password they typed on the login screen."
**Spec status:** Approved (2026-09-20)
**Plan and tasks status:** Approved (2026-09-20)
**Implementation authorized:** 2026-09-20

## Part 1 · Spec

### Goal

Users can reveal and hide the password they typed on login to avoid typos. *(Confirmed)*

### Current situation

- Android: `PasswordField` always masks (`PasswordVisualTransformation()`) — `android/feature/auth/src/main/java/com/acme/auth/LoginScreen.kt`. *(Verified)*
- iOS: `SecureField` with no toggle — `ios/Acme/Features/Auth/LoginView.swift`. *(Verified)*

### Requirements

- **FR-01:** An eye button inside the password field toggles between hidden and visible. Starts hidden every time the screen opens.

**Out of scope (agreed):** registration and change-password screens.

### Mobile behavior

| Situation | Expected behavior |
| --- | --- |
| Screen reader | Button announced as "Show password" / "Hide password" with its current state |
| *Android:* recreation · background and return | Visibility state kept; text kept |
| Touch target | At least 48 dp (Android) / 44 pt (iOS) |

**Not applicable:** loading, errors, network, back, process death (starts hidden by FR-01), store declarations.

### Acceptance criteria and how they are checked

| Criterion | Given / when / then | Level | Platform |
| --- | --- | --- | --- |
| AC-01 · FR-01 | Typed password, tap eye → visible; tap again → hidden | UI | Android, iOS |
| AC-02 · FR-01 | Screen reopened → hidden | UI | Android, iOS |
| AC-03 · FR-01 | TalkBack / VoiceOver → label and state announced | Device | Android, iOS |

### Decisions

None pending. Icon from each platform's design system (Material `Visibility`, SF Symbol `eye`). *(Confirmed)*

---

## Part 2 · Plan and tasks

### Changes

| Component or path | Action | Serves |
| --- | --- | --- |
| `LoginScreen.kt` · `PasswordField` | modify: `rememberSaveable` flag + trailing `IconButton` | FR-01 |
| `LoginView.swift` | modify: `@State` flag switching `SecureField` / `TextField` | FR-01 |

**Verification commands:** `./gradlew :feature:auth:testDebugUnitTest :app:assembleDebug` · `xcodebuild test -scheme Acme -destination 'platform=iOS Simulator,name=iPhone 15'` (from `.github/workflows/`). Release-build check not required (UI only, no new library).

### Tasks

- [x] **T-01 · Android toggle** — Serves FR-01, AC-01–03 · Validation: Compose UI test + TalkBack on device
  - Result (2026-09-21): UI test passes; TalkBack reads "Show password, button". Rotation keeps the state.
- [x] **T-02 · iOS toggle** — Serves FR-01, AC-01–03 · Validation: XCUITest + VoiceOver on device
  - Result (2026-09-21): XCUITest passes; VoiceOver reads "Show password, button".

### Validation table

| Criterion | Evidence | Platform | Result |
| --- | --- | --- | --- |
| AC-01 | `PasswordFieldTest` | Android | PASSED |
| AC-01 | `LoginUITests.testTogglePassword` | iOS | PASSED |
| AC-02 | Same tests, screen reopened | Android, iOS | PASSED |
| AC-03 | TalkBack, Pixel 6 | Android | PASSED |
| AC-03 | VoiceOver, iPhone 13 | iOS | PASSED |

**Closed:** all rows passed, no release items — marked Done without a sign-off question.
