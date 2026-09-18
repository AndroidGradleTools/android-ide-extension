# Android Gradle Tools — Build, ADB, Logcat & AI

![Version](https://img.shields.io/badge/version-1.1.0-blue)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
![Stack](https://img.shields.io/badge/Android-Gradle%20%7C%20ADB-3ddc84?logo=android&logoColor=white)
![VS Code](https://img.shields.io/badge/VS%20Code-1.85.0+-purple)

**Android development in VS Code and Cursor.** Build, install, debug, and inspect Android apps with Gradle, ADB, Logcat, emulators, diagnostics, and AI agent tools. Android Gradle Tools brings device targeting, APK caching, build variants, JDWP attach debugging, release signing, and an AI-ready Android toolchain into one editor surface—without juggling `./gradlew`, `adb`, `logcat`, and terminal tabs.

**[Install from the Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=AndroidGradleTools.android-ide-extension)** or search **Android Gradle Tools** in the Extensions view.

Install directly from a terminal:

```bash
code --install-extension AndroidGradleTools.android-ide-extension
```

![Android Gradle Tools: Gradle side panel with multi-root projects, device target, build actions, variant selection, Logcat with filters, and Android Build output](https://raw.githubusercontent.com/AndroidGradleTools/android-ide-extension/main/media/screenshots/overview.webp)

---

## Android workflows covered

- Android Gradle build, install, deploy, clean, and sync from VS Code or Cursor.
- ADB device targeting for physical Android devices, wireless ADB, remote ADB, and Android Emulator / AVD workflows.
- Android Logcat filtering by package, tag, level, crash, stack trace, text, and regex, with local clear separate from confirmed device-buffer clearing.
- APK cache browsing, reinstall, rollback, comparison, and sharing for successful Android builds.
- Android build variants, Gradle modules, multi-root workspaces, release signing, and JDWP attach debugging.
- An AI Toolchain for optional official Android CLI skills, structured diagnostics, local reviewable context bundles, and extension-owned Android development skills.

---

## Why use it?

Android Gradle Tools sits between terminal-only Android development and a full Android Studio session: light enough to stay inside VS Code, structured enough for real Android Gradle / ADB / Logcat iteration, and carefully tuned so routine Android app workflows stay obvious.

| Workflow need | Android Gradle Tools | Manual terminal loop |
|---|---|---|
| Build / install | Pick Android Gradle root, module, variant, and device in one panel | Re-type Gradle tasks, `adb` serials, and `cd` paths |
| Device targeting | Live Android device / emulator picker with auto-selection | `adb devices`, emulator checks, serial flags |
| Logcat | Integrated Android Logcat filters, presets, exports, stack-trace links | Long `adb logcat` pipelines |
| Gradle daemons | Isolated `GRADLE_USER_HOME` by default, idle cleanup | Shared daemon pool unless configured manually |
| APK history | Browse cached successful outputs | Hunt through `build/outputs/apk` |
| AI assistant context | Reviewable local evidence bundle plus Android skills for Codex, Claude, Copilot, Gemini, and Cursor | Manual screenshots, UIAutomator dumps, Logcat scans, and build-log summaries |

It also coexists cleanly with Android Studio. By default, extension builds use a dedicated `GRADLE_USER_HOME`, so panel builds do not share Studio's daemon pool. Idle daemons are stopped on a configurable timer with `./gradlew --stop`, scoped to the extension Gradle home unless you opt out of isolation.

## What you get

### Build and deploy

- Build, install, deploy, clean, and sync Android Gradle projects from the Android side panel.
- Pick the active Android Gradle root, module, and build variant without memorizing task names.
- Run **Cold AVD + Install**: assemble while an offline emulator starts, then install when it finishes booting.
- Launch the app after install using the detected `applicationId` / `namespace`, with a manual override when needed.
- Cache successful APK outputs for rollback, comparison, sharing, or re-install.

### Devices and variants

- Switch physical devices and emulators from the Build card or status bar.
- Auto-select a sensible target when devices connect, reconnect, or disappear.
- Discover variants from Gradle and cache them per module with validation that avoids surprise background Gradle runs.
- Reuse variant discovery results when Gradle inputs are unchanged, including when the same project is reopened under a different VS Code workspace identity.
- Keep multi-root workspaces explicit with a dedicated Gradle root picker.

### Logcat and build output

- Open Android Logcat from any folder—or with no folder open—in a dedicated webview with level, tag, package, crash, stack-trace, text, and regex filters.
- Prefix a tag with `-` (for example, `-OkHttp`) to exclude matching tags; combine exclusions with ordinary include tags when narrowing noisy streams.
- Restart Logcat without clearing the device-wide buffer by default; use the overflow **Clear device buffer...** action when you intentionally want `adb logcat -c`.
- Save frequent filter combinations as presets and export filtered output as `.txt` / `.log`.
- Open stack traces such as `File.kt:42` directly in the editor.
- End builds with a structured summary: result, duration, APK path, and extracted failure reasons.
- Keep full errors available through **Android: Show Full Build Errors** when a build fails.

### Release, debug, and remote workflows

- Set up release signing from the panel: pick or generate a keystore, detect aliases, store passwords in VS Code SecretStorage, and inject `signingConfigs` into Gradle files with a diff preview.
- Attach the Android JDWP debugger to a running Java / Kotlin process through local `adb` port forwarding.
- Use Wireless Remote ADB over same Wi-Fi, VPN / mesh, or an internet relay backed by [`adb-relay-android`](https://github.com/AndroidGradleTools/adb-relay-android) and [`adb-relay-jvm`](https://github.com/AndroidGradleTools/adb-relay-jvm).

### AI Toolchain

- Check the optional [official Android CLI](https://developer.android.com/tools/agents/android-cli) in the same local or Remote-SSH extension host that runs Gradle and ADB.
- List and search Google-maintained official Android skills without changing the environment; initialize or update them only after an explicit click and modal confirmation.
- Export diagnostics as stable schema-v1 JSON, including whether the extension host is local or remote.
- Run **Android: Create Agent Context Bundle** to collect bounded, redacted diagnostics, focused build errors, Logcat, a selected-device screenshot, and structured UI layout evidence into extension storage. The command opens `context.md` for review and never uploads or invokes a model.
- Use the **Lab** action in the Android panel’s Build toolbar—or run **Android: Inspect AppFunctions (Experimental)**—to open an auto-running AppFunctions inspector. It keeps compile SDK, Jetpack/KSP setup, service metadata, KDoc coverage, device recovery, and structured identifiers, descriptions, parameters, and response schemas together in the panel; it never builds, installs, enables, disables, or executes a function.
- Keep official Android skills separate from the four extension-owned skills below. Official skills are managed by Android CLI; extension skills are symlinked into detected agent folders by this extension.
- Link bundled extension skills into detected Codex, Claude, Copilot, Gemini, and Cursor home folders from panel settings.
- Use `android-screen-inspector`, `android-build-triage`, `android-logcat-debugger`, and `android-device-lab` for adb-first screen reads, build failure diagnosis, runtime log analysis, and device / emulator verification.
- Keep the extension as the source of truth: enabled agents receive extension-owned symlinks, disabling removes only those symlinks, and provider rows stay hidden until the matching agent folder exists.
- Replace broken or third-party skill symlinks only after an explicit confirmation, while preserving real user-owned files.
- Discover AI Toolchain settings from a dismissible panel banner above the CPU / RAM dock; dismissals stay quiet for 7 days unless a new agent folder appears.

### Panel polish

- Keep the main Android panel available outside Gradle projects: device, wireless ADB, Logcat, system logs, settings, and AI Toolchain remain active while project-only actions are clearly disabled.
- Configure the extension from a visual panel settings sheet tuned for the easiest possible setup, with a readable dark surface above the panel instead of hand-editing JSON for common workflows.
- Use a shared Material 3 / JetBrains-style visual language across the Android panel, Logcat, APK cache, settings sheet, wireless guide, menus, and popovers.
- Track local JVM CPU and RAM with a compact telemetry dock powered by `jps`, including process count, total CPU / memory, separate sparklines, and a hide/show control.
- Show the active device in the status bar.
- Use a polished Build surface with stable action-button sizing, softer section borders, smoother loading skeletons, clearer Gradle busy labels, and elapsed time aligned next to **Cancel**.
- Submit crash reports only after reviewing a sanitized payload in the browser; nothing is sent automatically.

## Get started

1. Open the **Android** side panel from the activity bar, or run **Android: Open side panel**. A Gradle project is optional for device, Logcat, wireless ADB, system log, settings, and AI Toolchain workflows.
2. For build workflows, open a standard Android Gradle project with `gradlew` or `gradlew.bat`.
3. Pick a Gradle root, module / variant, and device.
4. Click **Install**, **Compile**, **Clean**, or **Sync Gradle**.
5. Open **Logcat** from the panel or run **Android: Logcat**; use `-TagName` to suppress noisy tags.
6. Optional: open **AI Toolchain** in panel settings to check Android CLI and manage official or extension-owned Android skills.

If setup behaves strangely, run **Android: Run Diagnostics** and open **Android: Show System Gradle Log** to inspect the generated preflight report. Use **Android: Export Diagnostics as JSON** for a structured report, or **Android: Create Agent Context Bundle** when another developer or agent needs reviewable local evidence.

## Requirements

- VS Code or Cursor compatible with VS Code `^1.85.0`.
- A standard Android Gradle project with a Gradle Wrapper is required for build, install, signing, and Gradle diagnostics—not for standalone device and Logcat tools.
- `adb` on `PATH`, or an Android SDK path configured in the extension.
- Android Emulator is optional; when configured, the device picker can start AVDs.
- Android CLI is optional. Install it from the [official Android agent tools documentation](https://developer.android.com/tools/agents/android-cli) only when you want Google-maintained skills; existing Gradle, ADB, diagnostics, bundles, and extension skills do not require it.
- A JDK for Gradle. You can set it from the panel settings or `android-ide-extension.gradleJavaHome`.

## Commands

| Command | Purpose |
|---|---|
| **Android: Open side panel** | Open the main Android workflow surface |
| **Android: Build Project** / **Android: Build and Install** | Compile or install the selected target |
| **Android: Clean Project** / **Android: Sync Gradle project** | Run common Gradle maintenance flows |
| **Android: Build Variants** / **Android: Pick Gradle module** / **Android: Pick Gradle root** | Control active Gradle context |
| **Android: Select Device** | Pick a device or emulator |
| **Android: Run Diagnostics** | Print a preflight report for Gradle root, SDK / ADB, JDK, device, AVD, module, and variant setup |
| **Android: Export Diagnostics as JSON** | Save the same diagnostics as a local schema-v1 JSON document with execution-host context |
| **Android: Create Agent Context Bundle** | Create a bounded, redacted local evidence bundle and open `context.md` for review |
| **Android: Inspect AppFunctions (Experimental)** | Audit readiness and inspect registered function contracts on the selected device without execution |
| **Android: Logcat** | Open the integrated Logcat viewer |
| **Android: Open cached APK builds** | Browse successful cached outputs |
| **Android: Set up release signing** | Create or select signing configuration |
| **Android: Attach Debugger to Process** | Attach JDWP debugger to a running app process |
| **Android: Wireless / remote ADB (guided setup)** | Configure remote device access |
| **Android: Stop Gradle daemons** | Stop extension-scoped Gradle daemons |

## Settings

Most setup happens from the Android panel gear. The settings UI is tuned to make configuration as easy as possible: choose a JDK, point to an Android SDK, tune Gradle daemon cleanup, manage the AI Toolchain, configure Remote ADB, and adjust panel polish without leaving the workflow.

You can also search **Android Gradle Tools** in VS Code Settings when you prefer the full settings view.

| Area | Key settings |
|---|---|
| Gradle tasks | `installTask`, `buildTask`, `defaultGradleModule`, `gradleExtraArgs` |
| Gradle runtime | `gradleRunner`, `gradleJavaHome`, `buildOutputChannel` |
| Daemon hygiene | `gradleIsolateUserHome`, `gradleUserHome`, `gradleDaemonIdleStopMinutes`, `stopGradleDaemonsOnDeactivate` |
| Android SDK / device | `androidSdkPath`, `emulatorLaunchArgs`, `showStatusBarDevice` |
| Post-install launch | `launchAppAfterInstall`, `launchApplicationId` |
| Wireless Remote ADB | `remoteAdbRelayUrl`, `remoteAdbListenHost`, `remoteAdbLocalPort` |
| AI Toolchain | `androidCliPath` |
| Privacy | `crashReportEnabled` |

Sensible defaults work for typical debug workflows. The most important default is `gradleIsolateUserHome = true`, which keeps extension Gradle daemons separate from Android Studio and your normal `~/.gradle` pool.

## Frequently asked questions

### Can I build Android apps in VS Code?

Yes. Open a standard Android Gradle project and use Android Gradle Tools to select a Gradle root, module, build variant, and device, then build, install, or launch the app from the Android panel. Android Studio remains useful for specialized profilers and design tooling, but it is not required for the routine Gradle-to-device loop.

### How do I view Android Logcat in VS Code?

Run **Android: Logcat** or open Logcat from the Android panel, even when no Gradle project is open. You can filter by package, tag, level, crashes, stack traces, text, or regular expressions; prefix a tag with `-` to exclude it, then export the filtered output when you need to share or inspect it elsewhere.

### Can VS Code launch an Android emulator?

Yes. The device picker discovers configured Android Virtual Devices and can start an AVD before installing an app. **Cold AVD + Install** builds while the emulator boots, then installs after the device is ready.

### Can AI agents inspect an Android app and device?

Yes. The optional AI Toolchain manages official Android CLI skills and extension-owned Android development skills. **Android: Create Agent Context Bundle** collects bounded, redacted diagnostics, focused build errors, Logcat, a selected-device screenshot, and structured UI evidence locally for review before sharing with an agent.

### How can I support Android Gradle Tools?

If the extension improves your Android workflow, leave an honest [Marketplace rating or review](https://marketplace.visualstudio.com/items?itemName=AndroidGradleTools.android-ide-extension), or report a reproducible problem through [GitHub issues](https://github.com/AndroidGradleTools/android-ide-extension/issues). The extension does not interrupt your work with review prompts.

## Privacy and crash reporting

Crash reporting is opt-in at the point of submission. When an unexpected extension error is caught, Android Gradle Tools can open a browser page with a sanitized payload for review. Nothing leaves your machine until you press **Submit**.

Crash reporting can be turned off from the panel settings or VS Code Settings.

The bundled extension skills are local and adb-first. Screen inspection is read-only, and the skills do not send data to any network service by themselves.

Agent Context Bundles are also local. They are written under the extension's global storage, are limited and redacted for common secret assignments/query values, and open in the editor with a review-before-sharing warning. A bundle may still contain application text, device screenshots, paths, package ids, serials, or stack traces, so inspect it before copying its folder path or revealing it to another tool.

## Release notes

### 1.1.0 - 2026-09-18

- Kept the Android panel and standalone device workflows available without a Gradle project, including device selection, wireless ADB, Logcat, system logs, settings, and AI Toolchain; project-only actions remain visibly unavailable.
- Added negative Logcat tag filtering with `-Tag` tokens. Exclusions are case-insensitive, win over positive matches, and round-trip through existing presets.
- Expanded the experimental AppFunctions Lab with structured function identifiers, descriptions, parameters, referenced schemas, and response types while retaining bounded raw JSON as the authoritative fallback.

### 1.0.1 - 2026-08-28

- Added the experimental **AppFunctions Lab** as a compact **Lab** action in the existing Build toolbar and as **Android: Inspect AppFunctions (Experimental)** in the Command Palette.
- Added an auto-running full-panel inspector for compile SDK 36+, Jetpack/KSP dependencies, service entry points, manifest metadata, KDoc coverage, project context, selected-device recovery, and registered metadata.
- Kept device discovery read-only and pinned to the explicitly selected serial. It skips missing or offline targets and never builds, installs, launches an AVD, changes function state, or executes a function.
- Bounded metadata again after JSON formatting and prevented nested local `.worktrees/` content from entering production VSIX packages.

[Full changelog](CHANGELOG.md)
