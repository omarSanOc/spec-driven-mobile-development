# PROJECT CONTEXT

**Last verified:** 2026-09-12 · `a41c9e2`
**Status:** Approved (2026-09-12)

## Platforms

- **Technology:** Flutter 3.x (Dart) — `pubspec.yaml`
- **Target platforms:** Android, iOS (phones only) — `android/app/build.gradle.kts`, `ios/Runner.xcodeproj` (`TARGETED_DEVICE_FAMILY = 1`)
- **Minimum OS:** Android 7.0 (`minSdk 24`), iOS 15.0 — same files

## Architecture

- **Layers:** feature-first; `lib/features/<feature>/{data,domain,presentation}` — `README.md#architecture`
- **State management:** `flutter_bloc` (Cubits) — `pubspec.yaml`, `lib/features/auth/presentation/login_cubit.dart`
- **Navigation:** `go_router`, routes in `lib/app/router.dart`
- **DI:** `get_it` registrations in `lib/app/di.dart`
- **Design system:** `lib/design_system/` (`AcmeButton`, `AcmeTextField`, `AcmeResultView`)
- **Reference feature:** `lib/features/auth/` (login)

## Data

- **Networking:** `dio` through `lib/core/network/api_client.dart`; contract in `docs/api/openapi.yaml`
- **Persistence:** `shared_preferences` for flags; tokens in `flutter_secure_storage` — `lib/core/storage/`
- **Environments:** `--dart-define-from-file=env/<flavor>.json` — `README.md#environments`

## Tests

- `flutter_test`, `bloc_test`, `mocktail` — `pubspec.yaml` (dev_dependencies)
- Unit and widget tests in `test/`, mirroring `lib/`; no integration tests yet

## Commands (verified)

| Purpose | Command | Requires |
| --- | --- | --- |
| Static analysis | `flutter analyze` | — |
| Unit + widget tests | `flutter test` | — |
| Build Android | `flutter build apk --debug --dart-define-from-file=env/dev.json` | Android SDK |
| Build iOS | `flutter build ios --no-codesign --dart-define-from-file=env/dev.json` | macOS + Xcode |

Source: `.github/workflows/ci.yml`.

## Conventions

- **Feature docs:** `docs/features/<name>/`, FR/AC IDs
- **Identifiers and comments:** English
- **Dependency policy:** new packages need approval — `CONTRIBUTING.md`
- **Git:** branches `feature/<name>`, Conventional Commits — `CONTRIBUTING.md`

## Mobile baseline

| Concern | Current behavior | Evidence |
| --- | --- | --- |
| Lifecycle / process death | Not handled: no state restoration | no `restorationScopeId` in `lib/app/app.dart` |
| Orientation | Portrait only | `AndroidManifest.xml`, `Info.plist` |
| Back navigation | Default `go_router` pop; no `PopScope` anywhere | search of `lib/` |
| Theming / dark mode | Follows the system (`themeMode: ThemeMode.system`) | `lib/app/app.dart` |
| Accessibility | Design-system widgets expose `Semantics` labels | `lib/design_system/acme_text_field.dart` |
| Offline / cache | Not supported; errors shown on failure | `lib/core/network/api_client.dart` |
| Analytics | `Analytics.track(name, props)`; no personal data allowed | `lib/core/analytics/analytics.dart`, `docs/privacy.md` |
| Localization | ARB files, English and Spanish | `lib/l10n/` |
