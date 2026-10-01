# React Native (JavaScript/TypeScript), bare or Expo

Also read `android.md` and `ios.md` for host details. Check the React Native /
Expo SDK versions in `package.json` before relying on any API described here.

## Detection

- `package.json` depending on `react-native`; Expo if it depends on `expo` or
  has `app.json`/`app.config.(js|ts)`. Native folders `android/` and `ios/`
  (absent in Expo managed/CNG projects until prebuild). Monorepo tools
  (Nx, Turborepo, Yarn workspaces).
- Package manager from the lockfile: `package-lock.json`, `yarn.lock`,
  `pnpm-lock.yaml`, `bun.lockb`. Use that one only.

## What to inspect (Stage 1)

- **Architecture:** feature folders, hooks, services, stores; the reference feature.
- **State:** Redux Toolkit, Zustand, MobX, Jotai, Context; server state with
  TanStack Query or RTK Query.
- **Navigation:** React Navigation (stack/native-stack/tabs) or Expo Router
  (file-based routes in `app/`); deep link config.
- **Native layer:** the New Architecture is the default in recent React
  Native and Expo SDK versions and is no longer optional in the newest ones —
  check the version in `package.json` (and `newArchEnabled` in
  `android/gradle.properties`, Podfile settings or Expo config on older
  versions) instead of assuming. Turbo Modules, Fabric components, Expo
  Modules, and any legacy bridge modules that rely on interop.
- **Data:** fetch/axios; AsyncStorage, MMKV, SQLite, WatermelonDB;
  expo-secure-store / react-native-keychain.
- **Config:** env handling (`react-native-config`, Expo `extra`, `EXPO_PUBLIC_*`),
  EAS build profiles, OTA updates (e.g. expo-updates) and runtime version policy.
- **Quality:** TypeScript strictness, ESLint, Prettier.

## Mobile specifics

- **Lifecycle:** `AppState` (active, background, inactive on iOS).
- **State retention:** JS state is lost when the process dies; persistence is
  explicit (e.g. persisted store). Android activity recreation also applies.
- **Back:** `BackHandler` for Android hardware back; iOS swipe via native-stack
  `gestureEnabled`; `beforeRemove` / `usePreventRemove` to guard unsaved
  changes on both.
- **Safe areas and keyboard:** `react-native-safe-area-context`;
  `KeyboardAvoidingView` behaves differently per platform (`padding` vs `height`).
- **Accessibility:** `accessibilityLabel`, `accessibilityRole`,
  `accessibilityState`, font scaling (`allowFontScaling`, `maxFontSizeMultiplier`),
  `AccessibilityInfo.isReduceMotionEnabled()`.
- **Theming:** `useColorScheme` / `Appearance`; the navigation and UI library
  themes must follow it if the app supports dark mode.
- **Platform code:** `Platform.OS`, `.ios.tsx`/`.android.tsx` files — each one
  needs its counterpart or a shared fallback.
- **OTA updates:** a JS change shipped over the air must remain compatible with
  the native binary already installed.

## Tests and commands

- Jest with React Native Testing Library for units, hooks and components;
  MSW or fetch mocks for network error paths.
- E2E with Detox or Maestro only if already configured (device/emulator/simulator).
- Typical commands (with the project's package manager; verify scripts in
  `package.json`): `test`, `lint`, `tsc --noEmit`, `npx expo run:android` /
  `run:ios` or `npx react-native run-android` / `run-ios`,
  `cd ios && pod install` (macOS) for bare projects.

## Pitfalls to flag

- Logic inside components instead of testable hooks/services.
- Unhandled promise rejections swallowing errors.
- Secrets in the JS bundle (anything in the bundle is readable).
- A native module added for one platform only.
- Expo managed limits: a native change may require a development build or prebuild.
