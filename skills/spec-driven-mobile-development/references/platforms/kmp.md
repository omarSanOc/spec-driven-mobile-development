# Kotlin Multiplatform (KMP), with Compose Multiplatform or native UIs

Also read `android.md` and `ios.md`: KMP code runs inside both hosts and
inherits their lifecycles, back behavior and safe areas.

## Detection

- Gradle applying `org.jetbrains.kotlin.multiplatform` (`kotlin("multiplatform")`);
  source sets `commonMain`, `androidMain`, `iosMain` (and tests).
- `iosApp/` Xcode project consuming a shared framework (static or dynamic, via
  direct integration, CocoaPods or SPM).
- UI mode: Compose Multiplatform (`org.jetbrains.compose` plugin, UI in
  `commonMain`) **or** shared logic only with SwiftUI/Compose UIs per platform.
  Establish which one — it changes where every UI requirement lives.

## What to inspect (Stage 1)

- **Modules and source sets:** Gradle modules; targets declared (`androidTarget`
  or the Android KMP library plugin, `iosArm64`, `iosSimulatorArm64`, `iosX64`,
  others); intermediate source sets; how the iOS framework is produced.
- **Architecture:** where state lives (shared ViewModels, state holders, plain
  classes), DI (Koin, manual, none), navigation (library or hand-written).
- **`expect`/`actual`:** existing declarations and what they wrap.
- **Data:** Ktor, kotlinx.serialization, SQLDelight/Room KMP, DataStore,
  settings libraries, secure store per platform.
- **Swift interop:** how flows and suspend functions reach Swift (SKIE,
  KMP-NativeCoroutines, callbacks, or none) if SwiftUI consumes shared code.
- **Resources:** Compose resources (`composeResources`), generated `Res` package,
  strings and fonts.
- **Theming:** with Compose Multiplatform, `isSystemInDarkTheme()` and the
  shared theme; with native UIs, each host's theming (see `android.md`, `ios.md`).

## KMP rules for PLAN and implementation

- New code goes to `commonMain` by default; every descent into `androidMain` /
  `iosMain` is justified in the PLAN.
- Before adding an `expect`, check whether the shared library already covers
  the need (Compose Multiplatform covers most UI, layout and insets).
- An `expect` and **both** `actual`s land in the same change; otherwise iOS
  stops compiling while Android still builds.
- A new dependency must publish artifacts for **every** declared target and is
  declared in the narrowest source set that needs it.
- Both targets must compile after every task: an Android build proves nothing
  about Kotlin/Native.

## Tests and commands

- `commonTest` runs on every target (`kotlin.test`); `androidUnitTest` /
  `androidHostTest` on the JVM; `iosTest` on Kotlin/Native (simulator, macOS only).
- `kotlinx-coroutines-test` is needed for coroutine tests unless the project
  deliberately uses another approach — check what it does.
- Fakes over mocks in `commonTest` (JVM-only mocking libraries do not run on
  Native). `ktor-client-mock` for network error paths.
- Typical commands (verify task names; they vary with the Android plugin in use):
  `./gradlew :shared:allTests`, `./gradlew :shared:testDebugUnitTest` or
  `:shared:testAndroidHostTest`, `./gradlew :shared:iosSimulatorArm64Test`,
  `./gradlew :shared:compileKotlinIosSimulatorArm64`,
  `./gradlew :shared:linkDebugFrameworkIosSimulatorArm64`,
  `./gradlew :androidApp:assembleDebug`.
- Some platform APIs (e.g. Keychain) are not available to Kotlin/Native test
  binaries without an app host; verify those by running the app.

## Pitfalls to flag

- Treating a green Android build as proof for iOS.
- Platform-only APIs leaking into `commonMain`.
- `@OptIn` of experimental APIs where the project forbids it.
- Shared UI checked visually on one platform only (insets, keyboard, gestures
  differ in each host).
