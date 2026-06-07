# Android Gradle Tools — Build, Install, Logcat

![Version](https://img.shields.io/badge/version-0.2.7-blue)
![Status](https://img.shields.io/badge/status-public%20beta-yellow)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
![Stack](https://img.shields.io/badge/Android-Gradle%20%7C%20ADB-3ddc84?logo=android&logoColor=white)
![VS Code](https://img.shields.io/badge/VS%20Code-1.85.0+-purple)

> **Public beta.** Android Gradle Tools is under active development for VS Code and Cursor. Feedback through [Marketplace reviews](https://marketplace.visualstudio.com/items?itemName=AndroidGradleTools.android-ide-extension) and GitHub issues directly shapes the roadmap.

**Command the Android build loop from the editor.** Build Gradle, pick a module / variant / device, install APKs, inspect Logcat, cache outputs, attach JDWP, prepare release signing, and expose device state to AI assistants through MCP without juggling `./gradlew`, `adb`, `logcat`, and terminal tabs. The panel is tuned to make the common Android workflow as easy to use as possible.

**[Install from the Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=AndroidGradleTools.android-ide-extension)** or search **Android Gradle Tools** in the Extensions view.

![Android Gradle Tools: Gradle side panel with multi-root projects, device target, build actions, variant selection, Logcat with filters, and Android Build output](https://raw.githubusercontent.com/AndroidGradleTools/android-ide-extension/main/media/screenshots/overview.webp)

---

## Why use it?

Android Gradle Tools sits between terminal-only Android work and a full Android Studio session: light enough to stay inside VS Code, structured enough for real Gradle / ADB iteration, and carefully tuned so routine actions stay obvious.

| Workflow need | Android Gradle Tools | Manual terminal loop |
|---|---|---|
| Build / install | Pick root, module, variant, and device in one panel | Re-type Gradle tasks, `adb` serials, and `cd` paths |
| Device targeting | Live device / emulator picker with auto-selection | `adb devices`, emulator checks, serial flags |
| Logcat | Integrated filters, presets, exports, stack-trace links | Long `adb logcat` pipelines |
| Gradle daemons | Isolated `GRADLE_USER_HOME` by default, idle cleanup | Shared daemon pool unless configured manually |
| APK history | Browse cached successful outputs | Hunt through `build/outputs/apk` |
| AI assistant context | Local MCP tools for screenshot and view hierarchy | Manual screenshots and UIAutomator dumps |

It also coexists cleanly with Android Studio. By default, extension builds use a dedicated `GRADLE_USER_HOME`, so panel builds do not share Studio's daemon pool. Idle daemons are stopped on a configurable timer with `./gradlew --stop`, scoped to the extension Gradle home unless you opt out of isolation.

## What you get

### Build and deploy

- Build, install, clean, and sync Gradle from the Android side panel.
- Pick the active Gradle root, module, and build variant without memorizing task names.
- Run **Cold AVD + Install**: assemble while an offline emulator starts, then install when it finishes booting.
- Launch the app after install using the detected `applicationId` / `namespace`, with a manual override when needed.
- Cache successful APK outputs for rollback, comparison, sharing, or re-install.

### Devices and variants

- Switch physical devices and emulators from the Build card or status bar.
- Auto-select a sensible target when devices connect, reconnect, or disappear.
- Discover variants from Gradle and cache them per module with validation that avoids surprise background Gradle runs.
- Keep multi-root workspaces explicit with a dedicated Gradle root picker.

### Logcat and build output

- Open Logcat in a dedicated webview with level, tag, package, crash, stack-trace, text, and regex filters.
- Save frequent filter combinations as presets and export filtered output as `.txt` / `.log`.
- Open stack traces such as `File.kt:42` directly in the editor.
- End builds with a structured summary: result, duration, APK path, and extracted failure reasons.
- Keep full errors available through **Android: Show Full Build Errors** when a build fails.

### Release, debug, and remote workflows

- Set up release signing from the panel: pick or generate a keystore, detect aliases, store passwords in VS Code SecretStorage, and inject `signingConfigs` into Gradle files with a diff preview.
- Attach the Android JDWP debugger to a running Java / Kotlin process through local `adb` port forwarding.
- Use Wireless Remote ADB over same Wi-Fi, VPN / mesh, or an internet relay backed by [`adb-relay-android`](https://github.com/AndroidGradleTools/adb-relay-android) and [`adb-relay-jvm`](https://github.com/AndroidGradleTools/adb-relay-jvm).

### AI assistant bridge

- Enable a local Streamable HTTP MCP server bound to `127.0.0.1`.
- Expose `android_screenshot` and `android_view_hierarchy` for the device selected in the panel.
- Let modern VS Code discover the server natively, or use the panel URL / workspace `.mcp.json` for Claude Code and other clients.
- Use a fixed `mcpPort` for stable URLs, or `0` to pick a free port each session.

### Panel polish

- Configure the extension from a visual panel settings sheet tuned for the easiest possible setup, with a readable dark surface above the panel instead of hand-editing JSON for common workflows.
- Track local JVM CPU and RAM with a compact telemetry dock powered by `jps`, including process count, total CPU / memory, separate sparklines, and a hide/show control.
- Show the active device in the status bar.
- Use a polished Build surface with stable action-button sizing, softer section borders, smoother loading skeletons, clearer Gradle busy labels, and elapsed time aligned next to **Cancel**.
- Submit crash reports only after reviewing a sanitized payload in the browser; nothing is sent automatically.

## Get started

1. Open a standard Android Gradle project with `gradlew` or `gradlew.bat`.
2. Open the **Android** side panel from the activity bar, or run **Android: Open side panel**.
3. Pick a Gradle root, module / variant, and device.
4. Click **Install**, **Compile**, **Clean**, or **Sync Gradle**.
5. Open **Logcat** from the panel or run **Android: Logcat**.
6. Optional: enable **MCP server** in panel settings for AI screenshot and view-hierarchy tools.

## Requirements

- VS Code or Cursor compatible with VS Code `^1.85.0`.
- A standard Android Gradle project with a Gradle Wrapper.
- `adb` on `PATH`, or an Android SDK path configured in the extension.
- Android Emulator is optional; when configured, the device picker can start AVDs.
- A JDK for Gradle. You can set it from the panel settings or `android-ide-extension.gradleJavaHome`.

## Commands

| Command | Purpose |
|---|---|
| **Android: Open side panel** | Open the main Android workflow surface |
| **Android: Build Project** / **Android: Build and Install** | Compile or install the selected target |
| **Android: Clean Project** / **Android: Sync Gradle project** | Run common Gradle maintenance flows |
| **Android: Build Variants** / **Android: Pick Gradle module** / **Android: Pick Gradle root** | Control active Gradle context |
| **Android: Select Device** | Pick a device or emulator |
| **Android: Logcat** | Open the integrated Logcat viewer |
| **Android: Open cached APK builds** | Browse successful cached outputs |
| **Android: Set up release signing** | Create or select signing configuration |
| **Android: Attach Debugger to Process** | Attach JDWP debugger to a running app process |
| **Android: Wireless / remote ADB (guided setup)** | Configure remote device access |
| **Android: Stop Gradle daemons** | Stop extension-scoped Gradle daemons |

## Settings

Most setup happens from the Android panel gear. The settings UI is tuned to make configuration as easy as possible: choose a JDK, point to an Android SDK, tune Gradle daemon cleanup, enable MCP, configure Remote ADB, and adjust panel polish without leaving the workflow.

You can also search **Android Gradle Tools** in VS Code Settings when you prefer the full settings view.

| Area | Key settings |
|---|---|
| Gradle tasks | `installTask`, `buildTask`, `defaultGradleModule`, `gradleExtraArgs` |
| Gradle runtime | `gradleRunner`, `gradleJavaHome`, `buildOutputChannel` |
| Daemon hygiene | `gradleIsolateUserHome`, `gradleUserHome`, `gradleDaemonIdleStopMinutes`, `stopGradleDaemonsOnDeactivate` |
| Android SDK / device | `androidSdkPath`, `emulatorLaunchArgs`, `showStatusBarDevice` |
| Post-install launch | `launchAppAfterInstall`, `launchApplicationId` |
| Wireless Remote ADB | `remoteAdbRelayUrl`, `remoteAdbListenHost`, `remoteAdbLocalPort` |
| MCP | `mcpEnabled`, `mcpPort`, `mcpAutoRegister` |
| Privacy | `crashReportEnabled` |

Sensible defaults work for typical debug workflows. The most important default is `gradleIsolateUserHome = true`, which keeps extension Gradle daemons separate from Android Studio and your normal `~/.gradle` pool.

## Privacy and crash reporting

Crash reporting is opt-in at the point of submission. When an unexpected extension error is caught, Android Gradle Tools can open a browser page with a sanitized payload for review. Nothing leaves your machine until you press **Submit**.

Crash reporting can be turned off from the panel settings or VS Code Settings.

The MCP server is local-only by default. It binds to `127.0.0.1`, exposes no auth token, and targets only the Android device / emulator selected in the panel.

## Release notes

### 0.2.7 - 2026-06-07

- Added JVM RAM telemetry alongside CPU: totals, per-process chips, and separate CPU / RAM sparklines.
- Smoothed the initial loading skeleton and added a centered icon + text state for non-Gradle workspaces.
- Added regression coverage for the refreshed telemetry dock, shimmer, and empty state.

### 0.2.6 - 2026-06-07

- Removed the optional mascot/activity feature and its related settings, assets, listeners, and hooks.
- Kept regression coverage to verify the removed feature stays out of shipped source surfaces.

[Full changelog](CHANGELOG.md)
