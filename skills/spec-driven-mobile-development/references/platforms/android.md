# Android (native: Kotlin/Java, Jetpack Compose or Views)

Behavior depends on `minSdk`, `targetSdk` and library versions. Read them from
the build files before relying on any API or platform default described here.

## Detection

- `settings.gradle(.kts)`, `build.gradle(.kts)` applying `com.android.application`
  (and `com.android.library` for modules or SDKs), no Kotlin `multiplatform`
  plugin, and not the `android/` host folder of a Flutter or React Native app
  (see SKILL.md, Stage 1.3).
- `gradle/libs.versions.toml` (version catalog) if present; `AndroidManifest.xml`.

## What to inspect (Stage 1)

- **Modules:** `settings.gradle` includes; feature/core/data module split and
  dependency direction; build-logic / convention plugins.
- **Architecture:** MVVM / MVI / Clean layers; `ViewModel` usage; use cases;
  repositories; mappers. Find the reference feature the project points to.
- **UI:** Compose, XML Views, or mixed; design system / theme; navigation
  (Navigation Compose, Navigation component with fragments, custom).
- **State:** `StateFlow`/`SharedFlow`/`LiveData`; how UI state and one-off events
  are modelled; `SavedStateHandle`/`rememberSaveable` usage.
- **DI:** Hilt, Koin, Dagger, manual, or none.
- **Data:** Retrofit/OkHttp/Ktor; serialization; Room/DataStore/SharedPreferences;
  EncryptedSharedPreferences or Keystore; WorkManager.
- **Config:** build types, product flavors, `buildConfigField`, environments,
  where base URLs and keys live; `allowBackup`, `networkSecurityConfig`.
- **Manifest:** permissions, `screenOrientation`, `configChanges`,
  `windowSoftInputMode`, exported components, deep link intent filters.

## Mobile specifics

- **Configuration changes** (rotation unless locked, font size, dark theme,
  language, multi-window resize) recreate the Activity unless `configChanges`
  says otherwise. `ViewModel` survives it; plain `remember` does not.
- **Process death:** in the background the process can be killed and restored
  later; only `SavedStateHandle` / `rememberSaveable` / persisted data survive.
  Test with "Don't keep activities" or by killing the app process in the
  background, then returning from recents.
- **Back:** system back button/gesture; `OnBackPressedDispatcher` /
  `BackHandler`. Predictive back applies depending on `targetSdk` and manifest
  opt-in — check the project. With no handler, back leaves the screen or app.
- **Edge-to-edge and insets:** recent `targetSdk` values enforce edge-to-edge;
  status bar, navigation bar, display cutout, IME insets.
- **Keyboard:** `windowSoftInputMode`, `imePadding()`, IME actions, focus.
- **Permissions:** runtime permissions, "don't ask again", revocation from
  settings; `POST_NOTIFICATIONS` on newer Android versions.
- **Accessibility:** TalkBack, `contentDescription`/semantics, touch targets
  (48dp), font scale up to 200%, "Remove animations" setting.
- **Theming:** dark theme (`isSystemInDarkTheme()`, `DayNight` themes); dynamic
  color (Material You) only if the app already uses it.
- **Background work:** WorkManager constraints, Doze, app standby.
- **Backups:** `allowBackup` / data extraction rules decide what leaves the device.
- **Release build (R8):** check `isMinifyEnabled` / `isShrinkResources` per
  build type and the keep rules (`proguard-rules.pro`, libraries' consumer
  rules). Minification breaks what works in debug: reflection-based
  serialization (Gson, Moshi reflection, Firestore `toObject`, Jackson)
  without `@SerializedName`/`@Keep` or keep rules, classes loaded by name,
  enums parsed by name, generic types in Retrofit, JNI. When evidence-rules
  requires it: `./gradlew :app:assembleRelease` (or a minified variant signed
  with the debug key), install it, and run the feature's path; read the stack
  trace with `mapping.txt` (`retrace`).
- **Google Play declarations:** the **Data safety** form in Play Console must
  reflect every data type the app (and its SDKs) collects or shares; update it
  when the feature adds analytics, identifiers or a data-collecting SDK.
  Restricted permissions need a Play Console declaration (e.g. background
  location, SMS/Call log, all-files access, `QUERY_ALL_PACKAGES`, exact alarms,
  foreground service types, photo and video access — check current Play
  policy). These are release items outside the repository.

## Tests and commands

- Local JVM tests in `src/test` (JUnit4/5, kotlinx-coroutines-test, Turbine, MockK,
  Robolectric — only what is on the classpath).
- Instrumented tests in `src/androidTest` (Espresso, Compose UI test) need an
  emulator or device.
- Screenshot tests (Paparazzi, Roborazzi) only if already configured.
- Typical commands (verify against the project): `./gradlew test`,
  `./gradlew testDebugUnitTest`, `./gradlew :app:assembleDebug`,
  `./gradlew :app:assembleRelease` (or `bundleRelease`),
  `./gradlew connectedDebugAndroidTest`, `./gradlew lint`, plus ktlint/detekt/
  spotless if configured.

## Pitfalls to flag

- State kept only in `remember` inside a screen that must survive recreation.
- One-off events modelled as state that replays after recreation.
- Coroutines launched outside a lifecycle-aware scope (leaks, duplicate work).
- Secrets in `BuildConfig` or `strings.xml` shipped in the APK.
- A feature verified only in debug that touches serialization, reflection or a
  new library (R8 can break it in release).
- A release build pointing at a test environment.
- A behavior verified on another platform and assumed on Android.
