# iOS (native: Swift or Objective-C, SwiftUI or UIKit)

Behavior depends on the deployment target, Swift version and frameworks in use.
Read them from the project before relying on any API described here. Building
and testing iOS requires macOS with Xcode; if unavailable, those checks are BLOCKED.

## Detection

- `*.xcodeproj` / `*.xcworkspace`, `Package.swift`, `Podfile`, `Cartfile`,
  `Project.swift` (Tuist), `project.yml` (XcodeGen), and not the `ios/` or
  `iosApp/` host folder of a Flutter, React Native or KMP app (see SKILL.md,
  Stage 1.3).
- Check whether the app also targets iPad (`TARGETED_DEVICE_FAMILY`), Mac
  Catalyst or visionOS: that changes the target platforms.

## What to inspect (Stage 1)

- **Targets and packages:** app target, extensions, local Swift packages and
  their dependency direction; schemes and configurations (Debug/Release, staging).
- **Architecture:** MVVM, TCA, VIPER, Clean, MVC; `ObservableObject` /
  `@Observable`; coordinators or routers. Find the reference feature.
- **UI:** SwiftUI, UIKit, or mixed; design system; navigation
  (`NavigationStack`, coordinators, `UINavigationController`).
- **Concurrency:** async/await, actors, `@MainActor`, Combine; Swift 6 strict
  concurrency settings.
- **DI:** initializer injection, environment, a container, or none.
- **Data:** URLSession or a networking library; Codable; SwiftData, Core Data,
  GRDB, UserDefaults, files; Keychain wrapper.
- **Config:** `.xcconfig` files, build settings, `Info.plist` (or generated
  Info.plist keys in `project.pbxproj`), entitlements, `PrivacyInfo.xcprivacy`,
  App Transport Security, URL schemes and associated domains.

## Mobile specifics

- **Lifecycle:** `scenePhase` / scene delegate; the app is suspended in the
  background and may be terminated without further notice. There is no
  Android-style recreation on configuration changes, but size classes, Dynamic
  Type and appearance changes re-render views.
- **State restoration:** `@SceneStorage`, `NSUserActivity`, persisted state; by
  default nothing survives termination.
- **Back:** edge swipe on navigation stacks; interactive dismissal of sheets
  (`interactiveDismissDisabled`); no system back button. A half-filled form
  needs a defined swipe/dismiss behavior.
- **Safe areas:** notch / Dynamic Island, home indicator, keyboard safe area;
  `ignoresSafeArea` usage.
- **Keyboard:** focus (`@FocusState`), submit labels, the hardware keyboard in
  the simulator masking on-screen keyboard behavior.
- **Permissions:** usage description strings are mandatory in Info.plist; a
  denied permission can only be changed in Settings; limited photo access.
- **Accessibility:** VoiceOver labels, traits and order; Dynamic Type including
  accessibility sizes; 44pt touch targets; Reduce Motion; Increase Contrast.
- **Theming:** light/dark appearance (`colorScheme`, asset catalog colors with
  dark variants); a forced appearance (`UIUserInterfaceStyle`) if the app sets one.
- **Background work:** BGTaskScheduler, background URLSession; strict limits.
- **Privacy:** privacy manifest, required-reason APIs, App Store privacy labels.

## Tests and commands

- XCTest and/or Swift Testing (`@Test`, `#expect`) for unit tests; XCUITest for
  UI tests; snapshot testing only if already present.
- Typical commands (verify scheme and destination):
  `xcodebuild test -scheme <Scheme> -destination 'platform=iOS Simulator,name=<Device>'`,
  `swift test` for local packages, SwiftLint/SwiftFormat if configured.
- Simulator control: `xcrun simctl` (boot, install, launch, terminate).
- Keychain and some system services behave differently, or are unavailable, in
  unit test bundles without a host app: verify by running the app.

## Pitfalls to flag

- UI updates off the main actor.
- Work started in `onAppear`/`task` that repeats or is not cancelled.
- Secrets in source, plist or xcconfig committed to the repo.
- Missing usage description strings (the app crashes on the permission request).
- A behavior verified on another platform and assumed on iOS.
