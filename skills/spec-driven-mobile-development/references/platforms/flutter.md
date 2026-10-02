# Flutter (Dart)

Also read `android.md` and `ios.md` for host details (permissions, manifest,
Info.plist, safe areas). Check the Flutter/Dart SDK constraints in
`pubspec.yaml` before relying on any API described here.

## Detection

- `pubspec.yaml` with `flutter:` under `dependencies`/`environment`; `lib/`,
  `android/`, `ios/`, `test/`, `integration_test/`. Melos or a `packages/`
  folder for monorepos. FVM (`.fvmrc`) if the SDK version is pinned.

## What to inspect (Stage 1)

- **Architecture:** feature-first or layer-first folders; Clean layers; the
  reference feature.
- **State management:** from dependencies — Bloc/Cubit, Riverpod, Provider,
  GetX, MobX, signals, or `setState` only. Follow what is there.
- **Navigation:** `go_router`, `auto_route`, Navigator 1.0/2.0; deep link setup.
- **DI:** get_it/injectable, Riverpod providers, constructor injection.
- **Data:** dio/http; json_serializable/freezed; drift/sqflite/isar/hive/
  shared_preferences; flutter_secure_storage.
- **Platform code:** MethodChannel/EventChannel, Pigeon, FFI, or plugins —
  and which native code exists in `android/` and `ios/`.
- **Config:** flavors, `--dart-define` / `--dart-define-from-file`, environment
  files; analysis_options.yaml lint rules; l10n setup (ARB files, `gen-l10n`).

## Mobile specifics

- **Lifecycle:** `AppLifecycleListener` / `WidgetsBindingObserver`
  (resumed, inactive, hidden, paused, detached).
- **State restoration:** `restorationScopeId`, `RestorationMixin`; without it
  nothing survives process death. Android activity recreation also applies.
- **Back:** `PopScope` (replaces deprecated `WillPopScope`); blocking pop also
  disables the iOS swipe-back gesture — define both behaviors. Android predictive
  back support depends on the Flutter version and manifest opt-in.
- **Safe areas and keyboard:** `SafeArea`, `MediaQuery` padding/viewInsets,
  `resizeToAvoidBottomInset`.
- **Accessibility:** `Semantics`, text scaling (`TextScaler`), contrast, tap targets,
  `MediaQuery.disableAnimationsOf`.
- **Theming:** `theme` / `darkTheme` / `themeMode` in `MaterialApp`
  (or `CupertinoApp`); `MediaQuery.platformBrightnessOf`.
- **Platform look:** Material vs Cupertino widgets; adaptive constructors.
  Decide per requirement whether iOS should look native.
- **Release build:** release mode is AOT-compiled, applies R8 to the Android
  host and plugins, and with `--obfuscate` renames Dart symbols (breaks logic
  based on `runtimeType.toString()` or enum/class names). `kDebugMode` /
  `assert` code is gone. When evidence-rules requires it: `flutter build apk
  --release` (or `flutter run --release`) and the iOS release build on macOS,
  then exercise the feature's path. Store declarations follow `android.md` and
  `ios.md`.

## Tests and commands

- Unit and widget tests in `test/` with `flutter_test`; `mocktail`/`mockito`,
  `bloc_test` if present; golden tests only if already used.
- Integration tests in `integration_test/` need a device, emulator or simulator.
- Typical commands (verify; prefix with `fvm` if used): `flutter analyze`,
  `flutter test`, `flutter test integration_test`, `dart run build_runner build`
  when code generation is used, `flutter build apk --debug`,
  `flutter build ios --no-codesign` (macOS only).

## Pitfalls to flag

- Business logic inside widgets (not unit-testable).
- Forgetting generated code (`*.g.dart`, `*.freezed.dart`) after model changes.
- `BuildContext` used across async gaps.
- A plugin that supports only one platform.
- Material-only UI where iOS parity was required.
- Assuming both Android and iOS are targets: check which platforms the app
  actually ships on (some Flutter apps ship on one only).
