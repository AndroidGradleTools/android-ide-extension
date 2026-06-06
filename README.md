# Android Gradle Tools — Build, Install, Logcat

![Version](https://img.shields.io/badge/version-0.2.2-blue)
![Status](https://img.shields.io/badge/status-public%20beta-yellow)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
![Stack](https://img.shields.io/badge/Android-Gradle%20%7C%20ADB-3ddc84?logo=android&logoColor=white)
![VS Code](https://img.shields.io/badge/VS%20Code-1.85.0+-purple)

> **Public beta — under active development.** Frequent releases for VS Code and Cursor, focused on Android workflow ergonomics, daemon hygiene, and Logcat fidelity. **[Marketplace reviews](https://marketplace.visualstudio.com/items?itemName=AndroidGradleTools.android-ide-extension)** and repository feedback directly inform the roadmap.

**A focused Android build workspace for VS Code and Cursor.** Build Gradle, pick module / variant / device, install, inspect Logcat, cache APKs, attach JDWP, sign releases, and expose screen / view-hierarchy tools to AI assistants through MCP — **by clicking, not by juggling `./gradlew`, `adb`, `logcat`, `uiautomator`, and terminal tabs**.

**[Install from the Marketplace](https://marketplace.visualstudio.com/items?itemName=AndroidGradleTools.android-ide-extension)** — search **Android Gradle Tools** in the Extensions view.

> **Coexists with Android Studio.** The extension uses a dedicated **`GRADLE_USER_HOME`** by default, so panel builds and Android Studio do not share a daemon pool. Idle daemons are terminated on a configurable interval via `./gradlew --stop`, scoped to the extension's home only. Configure under **Panel → Settings (⚙)** — `gradleIsolateUserHome`, `gradleUserHome`, `gradleDaemonIdleStopMinutes` — or see **Settings** below.

---

## Overview

Gradle, devices, and Logcat in a single side panel. Build output is parsed into a structured summary — result, duration, APK path, and extracted failure reasons — so day-to-day Android work stays inside the editor.

![Android Gradle Tools: Gradle side panel with multi-root projects, device target, build actions, variant selection, Logcat with filters, and Android Build output](https://raw.githubusercontent.com/AndroidGradleTools/android-ide-extension/main/media/screenshots/overview.webp)

**Highlights:** 🧩 **isolated Gradle** + **idle daemon stop** · ⚡ one-panel Gradle · 🎯 variants & modules · 📱 device-aware installs · 📋 Logcat plus structured build output · 🤖 MCP tools for AI assistants

---

## Why Android Gradle Tools?

Three ways to drive an Android project from VS Code — pick the loop that fits your day.

| | Android Gradle Tools | Terminal-only | Android Studio |
|---|---|---|---|
| Build / install loop | **One side panel** — module, variant, device, run | Type Gradle + adb commands | Full IDE workflow |
| Logcat | **Integrated filters + presets** | `adb logcat` pipes | Built-in |
| Gradle daemons | **Isolated by default** | Shared unless manually configured | Studio-managed |
| Multi-root workspace | **Explicit Gradle root picker** | Manual `cd` discipline | Usually one project focus |
| Cached APKs | **Browseable history** | Manual file hunting | Not the same workflow |
| AI device tools | **Local MCP server** | Manual screenshots / XML dumps | Not built in |

Android Gradle Tools sits in the middle: lighter than a full Android Studio session, much friendlier than raw terminal loops.

## Features

Highlights, with what you click instead of what you would otherwise type:

| Feature | Replaces the manual loop |
|---|---|
| **Gradle side panel** | `./gradlew assemble...`, `install...`, `clean`, `sync` from scattered terminals |
| **Module / variant / root pickers** | Remembering the exact Gradle path, flavor, and active workspace root |
| **Device and emulator targeting** | `adb devices`, serial flags, offline AVD checks, and cold-boot timing |
| **Logcat viewer** | Long `adb logcat` pipelines for level, package, tag, crash, regex, and export |
| **APK cache browser** | Hunting through `build/outputs/apk` after every successful run |
| **Release signing setup** | Hand-editing `signingConfigs`, aliases, passwords, and Gradle properties |
| **JDWP attach debugger** | Manual port forwarding and process lookup for Java / Kotlin debugging |
| **Wireless Remote ADB** | Cable / same-Wi-Fi-only device workflows |
| **MCP tools for AI assistants** | Manual screenshots and `uiautomator dump` when an agent needs device state |
| **Typing-aware Capybara companion** | Guessing whether editor, terminal, or AI assistant activity is still moving |

Also: isolated `GRADLE_USER_HOME`, idle daemon cleanup, structured build summaries, cached build errors, Android SDK path resolution, status-bar target display, post-install launch, and privacy-preserving crash reports.

| Highlight | What you get |
| :--- | :--- |
| **Unified workflow** | Build, clean, install, and sync Gradle from a single side panel. Module, variant, and device selection sit adjacent to the action buttons, so a typical iteration is one click rather than a sequence of Command Palette entries. |
| **Isolated Gradle, calmer daemons** | A dedicated **`GRADLE_USER_HOME`** keeps editor-side Gradle out of Studio's daemon pool by default. Idle daemons are terminated on a timer; **Stop Gradle daemons** in the panel performs an explicit cleanup. **Stop on window close** is available as an opt-in. The shared `~/.gradle` remains a single-setting fallback. |
| **Multi-root ready** | Manage **multiple Gradle roots** in one window with explicit project context, ideal for polyrepos and SDK-style workspaces. |
| **Smarter device targeting** | The active emulator or device is shown in the **Build** card and switched with a single click. **Auto-selection** picks a sensible target whenever hardware connects or reconnects. An optional **Android SDK path** (or environment / `local.properties`) aligns `adb` and `emulator` with the SDK when the editor's `PATH` differs from the login shell. |
| **Faster iteration** | Optional **APK cache on compile / install** keeps successful outputs handy for comparison, rollback, and sharing — without leaving the IDE. |
| **Release signing in-panel** | A **Sign chip** in the Build card switches keystores per build. Generate via `keytool`, save passwords to OS keychain, and auto-inject `signingConfigs` into `build.gradle.kts` or `build.gradle` with diff preview. |
| **Observable builds** | **Android Build** output ends with a structured summary — result, duration, APK path, and extracted failure reasons — alongside deprecation context and links to Gradle documentation. Discovery and validation route to a separate **System Gradle Log**, leaving the main build stream clean. A **JVM CPU** strip surfaces local Java processes (`jps`) with CPU% and a rolling sparkline while the panel is visible. |
| **First-class Logcat** | A dedicated Logcat viewer with multi-select **level**, **tag**, and **package** filters — autocomplete is sourced from the live stream, and PIDs are re-resolved in the background so application restarts continue to match. Stack-trace links open `File.kt:42` style references in the editor. Filtered output exports to `.txt`/`.log`, frequent filter combinations save as named **presets**, and shift-click row selection feeds **Copy selected** for sharing slices of the stream. Font size is adjustable. |
| **Wireless Remote ADB** | Debug a remote Android device over the internet (or a VPN/mesh) from VS Code — no cable, no shared Wi-Fi required. The panel shows tabbed setup for **Same Wi-Fi**, **Internet relay**, and **VPN/mesh** modes. In Internet relay mode the IDE downloads and installs **[adb-relay-android](https://github.com/AndroidGradleTools/adb-relay-android)** on the phone directly, and the **[adb-relay-jvm](https://github.com/AndroidGradleTools/adb-relay-jvm)** process acts as the relay host. |
| **AI assistant bridge** | Optional local MCP server exposes `android_screenshot` and `android_view_hierarchy` against the device selected in the panel. VS Code Chat / Copilot can discover it natively on modern VS Code, while Claude Code and other clients can use the generated URL or workspace `.mcp.json`. |
| **Capybara activity companion** | A compact pixel-art Capybara sits above the JVM CPU strip, reacts to typing and AI assistant activity, and can be collapsed to a one-line restore control. Optional local hooks and cross-window beacons are metadata-only and never read transcript contents. |

---

## Feature details

### Build and deploy

- **Gradle home & daemons** — default **isolated `GRADLE_USER_HOME`**, **idle** **`./gradlew --stop`**, and the **Stop Gradle daemons** action; wire **JDK for Gradle** in the same **Settings** sheet. Isolation and timing are controlled with `android-ide-extension.gradleIsolateUserHome`, `gradleUserHome`, `gradleDaemonIdleStopMinutes`, and related options.
- **Install, compile, and clean** from one panel — progress, cancellation, and elapsed time where you need them.
- **Gradle sync** via **Android: Sync Gradle project** from the panel, editor toolbar, or Command Palette.
- **Cached APK builds** — successful install / compile flows can copy APKs into a workspace cache. Open **Android: Open cached APK builds** for history with timestamps and duration metadata.

### Release signing

- **Sign chip** in the Build card header. **Default** = Gradle's own signing; saved signatures inject Gradle project properties through the Gradle process environment at build time (same mechanism as Studio's *Generate Signed Bundle/APK*, without putting passwords on the command line).
- **Setup `<Variant>` signature** overlay: pick an existing keystore (with **Detect aliases**) or **Generate new** via `keytool`. Passwords go to OS keychain via `SecretStorage`.
- **Auto-inject `signingConfigs`** into `build.gradle.kts` or `build.gradle` (Groovy DSL) with diff preview; idempotent. Credentials via **SecretStorage**, **`~/.gradle/gradle.properties`**, or project **`local.properties`**.
- Commands: **Android: Set up release signing**, **Android: New custom signature (local override)**.

### Variants and modules

- The Android side panel exposes module and variant as a compact **`module / variant`** breadcrumb pill in the **Build** card — click each half to open its picker (module picker / Build Variants picker). **Discover variants** and **Clear variant** sit in a `⋮` overflow menu next to the pill. Variant pick is **scoped to the current module** so it never overwrites your module selection.
- Searchable quick picks for **Gradle root**, **module**, and **build variant** (**Android: Build Variants**); selection is paused during active builds for consistent state.
- After **Android: Discover variants (Gradle)**, variant names are cached per module with debounced validation (no surprise Gradle runs in the background). While discovery runs, the panel shows a **circular progress** indicator and a busy row.

### Devices

- The **Build** card shows the active target as a **device pill** (glyph + name + chevron) — click it to open the device / AVD picker. The wireless ADB entry button sits next to it.
- Target **physical devices** or **emulators** without memorizing `adb` flags.
- **Cold AVD + Install** (panel): if you pick an AVD that is not running, **assemble** runs while the emulator starts and finishes booting, then **install** runs — see **System Gradle Log** for emulator output (no dedicated terminal tab).
- **Auto-selection** polls `adb` and picks a sensible default; if the current device disconnects, the extension falls back to the most recently active target. An explicit **offline AVD** choice stays selected until that emulator is up or you pick another target.

### Console and logs

- **Logcat** — `adb logcat` rendered in a webview with multi-select **level**, **tag**, and **package** filters (autocomplete from the live stream; PIDs re-resolve so application restarts continue to match), plus dedicated **Crash** and **Stack trace** category filters and **text / regex** message search with inline regex-error feedback. The workspace's Android `applicationId` is auto-filled as the package filter on open. Stack-trace links open `File.kt:42` style references in the editor. Filtered output **exports** to `.txt` / `.log`; filter combinations save as named **presets** (up to 10). The **Timeline** toggle adds millisecond timestamps, second-boundary separators, and dedups consecutive rows sharing the same tag for cleaner reading. Shift-click row selection with **Copy selected**, plus per-line copy. Font size is adjustable. Open from the panel or **Android: Logcat**.
- **Build output with summary** — every run can end with a structured **BUILD SUMMARY**: result, duration, APK path, and extracted failure reasons.
- **Full build errors** — use **View Full Errors** on the notification or **Android: Show Full Build Errors** for a read-only tab with compilation errors and Gradle details in context.

### Wireless Remote ADB (experimental)

- **Android: Start / Stop Remote ADB Bridge** — starts a local WebSocket mux bridge that exposes the connected device to a remote relay host. Settings: `remoteAdbRelayUrl`, `remoteAdbListenHost`, `remoteAdbLocalPort`; the session token is saved in VS Code SecretStorage when configured from the panel.
- **Panel — Wireless Remote ADB**: tabbed guide for **Same Wi-Fi**, **Internet relay**, and **VPN/mesh** setups with a live **●** status indicator.
- **Internet relay**: in relay mode the IDE lets you pick a connected device, then downloads and installs **[adb-relay-android](https://github.com/AndroidGradleTools/adb-relay-android)** on the phone with live progress. The relay host must run **[adb-relay-jvm](https://github.com/AndroidGradleTools/adb-relay-jvm)** (standalone process, not bundled).

### AI assistants (MCP)

- **Local Streamable HTTP MCP server** — enable **MCP server** in panel settings or set `android-ide-extension.mcpEnabled = true`. It binds to `127.0.0.1`, exposes no auth token, and targets whichever Android device / emulator is selected in the panel.
- **Tools:** `android_screenshot` captures the selected device; `android_view_hierarchy` dumps UIAutomator XML so agents can inspect what is on screen.
- **VS Code native discovery:** modern VS Code hosts discover the server through `contributes.mcpServerDefinitionProviders`; the extension resolves the server definition only after the HTTP listener is ready, avoiding stale URL races.
- **External clients:** the panel shows the endpoint URL (`http://127.0.0.1:<port>/mcp`). `mcpAutoRegister` can also maintain a workspace `.mcp.json` entry named `android-gradle-tools` for Claude Code. Use a fixed `mcpPort` for stable URLs; `0` picks a free port per session.

### Capybara typing companion

- **Capybara dock** — the Android panel includes a small pixel-art Capybara above the **JVM CPU** strip. It animates with editor typing, terminal activity, AI assistant activity, and optional cross-window local beacons.
- **AI activity detection** — when `npcAiActivity` is enabled, the watcher monitors timestamp changes in known assistant history / transcript files while the Android panel is visible. It does not read file contents.
- **Optional input hooks** — `npcAutoSetupHooks` can install append-only hooks for supported tools such as Claude Code, Codex, Gemini, Cursor, and Copilot. Hooks write bounded local activity beacons into extension global storage so the Capybara can react promptly.
- **Compact controls** — hide/show and settings are icon buttons; collapsed mode keeps a one-line **Capybara** label with a small restore-eye control. Banner text and display mode are configurable.

### Debug (experimental)

- **Android: Attach Debugger to Process** lists processes, forwards JDWP over `adb`, and starts an **Android JDWP Debugger** attach session; **Android: Detach Debugger** ends it. The panel exposes attach / disconnect and shortcuts to VS Code stepping and breakpoints for **Java** and **Kotlin**. Behavior is still maturing — see **[CHANGELOG.md](CHANGELOG.md)**.

### Polish

- **JVM CPU** (bottom of the Android panel): polls local JVMs with **`jps`**, shows PID, CPU % (from `ps` on macOS/Linux or Windows perf counters), and a rolling **total CPU** sparkline with layout tuned for the webview. Uses the same JDK resolution as Gradle (**JDK for Gradle** / `JAVA_HOME`); install a JDK if you see “jps not found”.
- Optional **status bar** device label, **editor title** actions, and **open app after install** via `MAIN/LAUNCHER` (with **monkey** fallback). Application id resolution follows **applicationId** / **namespace** from Gradle, including **suffix** rules when applicable.
- Shortcuts from the editor keep the Gradle webview aligned with your latest module, variant, and device choices.

### Crash reporting

- Optional one-click bug reporting with **strong privacy** — payload is sanitized locally and shown to you on a webpage; **nothing is sent until you press Submit**. Disable with **`android-ide-extension.crashReportEnabled = false`**.

---

## Get started

1. Open the folder that contains your **Gradle wrapper** (`gradlew` / `gradlew.bat`).
2. Open the **Android** side panel — activity bar icon, or **Android: Open side panel** (**⇧⌘P** / **Ctrl+Shift+P**).
3. Choose a **device**, then **Install**, **Compile**, **Clean**, or **Sync Gradle** as needed.
4. Open **Logcat** from the panel or run **Android: Logcat**.
5. Optional: **Android: Open cached APK builds** after a successful flow to inspect cached artifacts and timings.
6. Optional: enable **MCP server** in panel settings so VS Code Chat / Copilot / Claude Code can call Android screenshot and view-hierarchy tools.

---

## Before you install

Built for **standard Android Gradle** projects.

- Workspace includes **Gradle Wrapper** (`gradlew` / `gradlew.bat`).
- **adb** on `PATH` for devices, install, Logcat, and optional post-install launch.
- **Android Emulator** is optional; the device picker can start AVDs when configured.

---

## Settings

Search **Android Gradle Tools** in Settings to configure defaults such as:

- **install** / **assemble** tasks when no variant is picked
- **spawn** vs **task** Gradle runner and **Android Build** output channel behavior
- **JDK for Gradle** / **JAVA_HOME** and the **panel Settings (gear)** quick editor
- **Isolated `GRADLE_USER_HOME`:** `gradleIsolateUserHome` (default on), optional `gradleUserHome` override — keeps extension daemons separate from **`~/.gradle`** and Studio unless you disable isolation
- **Daemon shutdown:** **Stop Gradle daemons** command; **`gradleDaemonIdleStopMinutes`** (automatic `./gradlew --stop` after idle); optional **`stopGradleDaemonsOnDeactivate`** when the window closes; optional log/toast via **`gradleDaemonIdleStopLog`** / **`gradleDaemonIdleStopNotify`**
- **Android SDK path** (optional): absolute path to the SDK root so **`platform-tools/adb`** and **`emulator`** resolve when **`PATH`** in the IDE differs from your terminal
- **extra Gradle arguments** for every invocation
- **status bar** device visibility and **launch after install** (plus optional **application id** override); launch uses **`aapt dump badging`** to read the real `applicationId` from the installed APK when available
- **Wireless Remote ADB:** `remoteAdbRelayUrl`, `remoteAdbListenHost`, `remoteAdbLocalPort`; session token is stored in VS Code SecretStorage when saved from the panel
- **MCP server:** `mcpEnabled`, `mcpPort`, `mcpAutoRegister`; native VS Code discovery works on newer hosts, while URL / `.mcp.json` registration work for external clients
- **Capybara companion:** `npcAiActivity`, `npcAiActivityTools`, `npcAutoSetupHooks`, `npcCrossWindowBeacon`, `npcBannerText`, `npcBannerMode`
- **Crash reporting:** `crashReportEnabled` (default on) — when off, no notifications and nothing is logged to the **Android Errors** channel

Sensible defaults work for typical **debug** workflows — no configuration required to start.

---

## Release notes

**0.2.2** - **Logcat crash retention**: package-filtered Logcat sessions now keep already captured crash logs visible after the target app exits.

- **Fixed** - When `pidof <package>` returns empty after a crash, ANR, or force-stop, Logcat no longer re-filters the existing buffer down to zero lines.
- **Fixed** - Export keeps using captured historical package PIDs, so post-mortem logs remain exportable after the process dies.

**0.2.1** - **Typing-aware Capybara companion**: the Android panel now has a compact Capybara above the JVM CPU strip that reacts to editor, terminal, cross-window, and supported AI assistant activity.

- **Added** - Optional AI input hooks and local activity beacons for Claude Code, Codex, Gemini, Cursor, and Copilot.
- **Polished** - Capybara hide/show/settings controls use icons; collapsed mode keeps the restore eye compact.
- **Fixed** - Capybara canvas and settings are centered independently of the title label.

[Full changelog](CHANGELOG.md)
