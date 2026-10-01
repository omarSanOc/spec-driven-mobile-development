# Mobile guidelines for SDMD

Consider each point when writing the SPEC and apply the ones the feature
touches. When a behavior is not defined, ask before assuming it. These points
do not widen scope by themselves: only confirmed decisions become requirements
and acceptance criteria. Points that do not apply are recorded as `N/A` with
the reason.

Define every point for **each target platform** (the platforms the app ships
on, recorded in Stage 1). When there is more than one target and the expected
behavior differs, write the difference as expected behavior, never as a
tolerated deviation. The platform files (`platforms/*.md`) explain how each
point shows up in each technology. Notes marked *Android* or *iOS* apply only
when that platform is a target.

## Checklist

- **Lifecycle.** What happens when the app goes to the background, returns,
  a screen is recreated, or the process is killed. What happens to operations
  in progress. Lifecycles are not symmetric: *Android* can recreate a screen on
  a configuration change (font size, theme, language, rotation if allowed);
  *iOS* does not, but can terminate a suspended app under different criteria.
- **State retention.** What must survive navigating away, returning, or
  reopening the app: forms, selections, scroll position, filters. Separate
  temporary UI state from persistent data.
- **Connectivity.** No connection, slow network, drops mid-operation, recovery.
  Separate first use (no data) from use with previously downloaded data.
- **Unstable connectivity / Offline-first.** If relevant, what is cached, for
  how long, what the user sees offline, how stale data is shown, and how
  pending changes sync. The SPEC decides what is kept and shown; the storage
  technology and where it lives are PLAN decisions. Never assume offline
  support exists or is wanted.
- **Persistence and consistency.** What is stored on the device and how it
  coexists with remote data: updates, duplicates, conflicts, compatibility with
  data saved by earlier app versions, and deletion (logout, uninstall, backup).
- **UI states.** Loading, content, empty (nothing yet), no results (filters
  matched nothing), error, success. Feedback for every action and a way to
  recover from errors where it makes sense.
- **Navigation and interruptions.** Back, cancel, leaving with unsaved changes,
  returning from another app, incoming calls or system dialogs, deep links and
  notifications if they are part of the flow. Back differs by platform:
  *Android* has a system back button/gesture (with predictive back) that can
  leave the app; *iOS* uses the edge swipe and sheet dismissal. Define each
  target's behavior, especially with a half-filled form.
- **Platform parity** (only with more than one target). Same behavior on all
  targets, or an explicit, intended difference (permissions, sharing,
  biometrics, system formats, platform conventions). If a capability exists on
  only one platform, define what the others do.
- **Interaction and forms.** Keyboard type, focus order, IME actions, the
  keyboard not covering fields or primary actions, validation timing and
  messages, visible actions. Repeated taps must not produce duplicate sends or
  operations.
- **Screens and visual adaptation.** Screen sizes (phones, tablets, foldables),
  supported orientations, system areas (status bar, navigation bar, notch or
  Dynamic Island, home indicator), large text / Dynamic Type. No clipped
  content, no unreachable controls. Shared screens are checked on every target.
- **Theming and appearance.** Light and dark mode, high-contrast settings,
  dynamic or brand colors if the app uses them. New UI follows the existing
  theme tokens; no hard-coded colors that break in one appearance.
- **Accessibility.** Screen readers (TalkBack and VoiceOver announce the same
  semantics differently), labels, focus order, contrast, touch target size,
  announcements of dynamic content, reduced motion (animations that respect the
  system setting). Never convey information only by color or only by a
  gesture; gestures need an accessible alternative.
- **Permissions and device capabilities.** Request only what is needed, at the
  moment it is needed, with context. Define behavior when denied, permanently
  denied, revoked in settings, or unavailable on the device.
- **Performance and resources.** Keep the UI responsive during network,
  storage and processing work. Consider memory, battery, data usage, images,
  long lists and pagination, startup time.
- **Background work.** When applicable, a task can be delayed, throttled or
  killed by the OS. Define how it resumes and how duplicate effects are
  avoided.
- **Privacy and security.** What is stored, transmitted or displayed, how it is
  protected, when it is deleted. No credentials, tokens or personal data in
  logs, analytics, crash reports or error messages. Screenshots/app switcher
  snapshots of sensitive screens if relevant.
- **Analytics and feature flags.** Only if the project uses them: which events
  the feature emits and with which properties (no personal data unless the
  privacy policy allows it), consent requirements, and whether the feature is
  behind a flag (name, default, behavior when off).
- **Languages and formats.** Translations, text length, dates, time zones,
  numbers, currency, units, right-to-left. System formatting can differ between
  platforms with the same locale; if a format must be identical, state it as a
  requirement.
- **Store and release constraints.** When relevant: store review rules, privacy
  declarations, minimum OS versions, staged rollout, forced update,
  over-the-air update compatibility, compatibility with older app versions
  still in use.
- **Mobile validation.** Test the relevant scenarios (interruptions, reopening,
  connectivity changes, permissions, screen sizes, large text, dark mode).
  Complement automated tests with device, emulator or simulator checks. For
  each scenario, record which target platform it was verified on, and what
  stayed unverified for lack of environment (macOS, Xcode, simulator, device).

## Mobile behavior table (SPEC)

This is the **single source** for the table in the SPEC. Copy the rows that
apply; omit rows tagged with a platform that is not a target; keep the rest
and write `N/A` with the reason when a row does not apply.

| Situation | Expected behavior |
| --- | --- |
| Loading or action in progress | |
| No data (first use / empty) | |
| No results for the current filters | |
| Invalid input | |
| Error or excessive wait (time limit and what the user sees) | |
| No connection or connection lost mid-operation | |
| Repeated taps on the primary action | |
| Cancel or go back — *Android:* system back / predictive back | |
| Cancel or go back — *iOS:* edge swipe / sheet dismissal | |
| Background and return | |
| *Android:* screen recreation (configuration change) | |
| Reopen after process death | |
| Large text / accessibility services on | |
| Dark mode / appearance change | |
| Differences between target platforms ("None" if identical; omit with a single target) | |
| Other applicable guideline points | |

Adapt rows to the project baseline (e.g. drop rotation if the app is locked to
portrait, but say so). Express results, not mechanisms.

## Features that talk to a network or hold a session

The moment a feature adds networking, a session, persistence or secrets, these
become part of its SPEC, PLAN and validation:

**SPEC**
- What each failure looks like, case by case — cases the user must tell apart
  (their fault vs. the app's vs. the server's), not one message per status code.
- What survives and for how long: sessions, tokens, downloaded data. What the
  user sees at the moment something expires.
- What is stored on the device and what is never stored (credentials).
  Whether it is included in device backups.
- What happens offline, separately for first use and for a device with a session
  or cached data.
- Language and uniformity of server messages: verify before promising to show them.
- Time limits: how long before the app gives up, and what the user sees.

**PLAN**
- Cancellation of in-flight requests when the screen goes away; duplicate send
  prevention; retry policy.
- Secure store per target platform (e.g. Keychain on iOS, Keystore-backed
  storage on Android).
- Secrets and base URLs kept out of source; how a build selects its environment
  and what prevents a release build pointing at a test environment.

**Validation**
- Error paths covered by unit tests with a fake transport, one test per case
  named in the SPEC.
- Persistence, process death and secure storage verified per target platform.
- Proof that credentials and tokens never appear in logs or error messages.
