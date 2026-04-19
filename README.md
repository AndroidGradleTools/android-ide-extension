# Android Gradle Tools — Build, Install, Logcat

![Version](https://img.shields.io/badge/version-0.1.20-blue)
![Status](https://img.shields.io/badge/status-public%20beta-yellow)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
![Stack](https://img.shields.io/badge/Android-Gradle%20%7C%20ADB-3ddc84?logo=android&logoColor=white)
![VS Code](https://img.shields.io/badge/VS%20Code-1.85.0+-purple)

> **Public beta, actively evolving.** We ship frequent updates focused on **performance**, **reliability**, and **quality-of-life** for Android developers in VS Code and Cursor. Your **[Marketplace reviews](https://marketplace.visualstudio.com/items?itemName=AndroidGradleTools.android-ide-extension)** and repository feedback directly influence the roadmap.

**One workspace for Gradle, devices, Logcat, and build output** — stay inside the editor you already use instead of bouncing between terminals and external tools.

**[Install from the Marketplace](https://marketplace.visualstudio.com/items?itemName=AndroidGradleTools.android-ide-extension)** — search **Android Gradle Tools** in the Extensions view.

---

## Overview

Gradle project control, streaming Logcat, and structured build diagnostics in a single, cohesive layout — tuned for daily Android engineering workflows.

![Android Gradle Tools: Gradle side panel with multi-root projects, device target, build actions, variant selection, Logcat with filters, and Android Build output](https://raw.githubusercontent.com/cuongnv126/android-ide-extension/main/media/screenshots/overview.webp)

**Highlights:** ⚡ one-panel Gradle · 🎯 variants & modules · 📱 device-aware installs · 📋 Logcat plus structured build output

---

## Why install it

| Highlight | What you get |
| :--- | :--- |
| **Unified workflow** | Run, build, clean, and sync **Gradle** from the Android side panel alongside **module** and **build variant** selection — fewer context switches, clearer state. |
| **Multi-root ready** | Manage **multiple Gradle roots** in one window with explicit project context, ideal for polyrepos and SDK-style workspaces. |
| **Smarter device targeting** | See the active **emulator or device**, switch targets quickly, and benefit from **automatic device selection** when hardware connects or reconnects. |
| **Faster iteration** | Optional **APK cache on compile / install** keeps successful outputs handy for comparison, rollback, and sharing — without leaving the IDE. |
| **Observable builds** | **Android Build** output with summaries, deprecation context, and links to Gradle documentation — plus a separate **System Gradle Log** so discovery and validation stay out of your main build stream. A **JVM CPU** strip lists local Java processes (via `jps`) with **CPU %** and a rolling aggregate sparkline (scaled to actual load, including near-idle) while the panel is visible. |
| **First-class Logcat** | Dedicated viewer with **level**, **tag**, **package**, and **text / regex** filters, color by severity, and connection status to the selected device. |

---

## Capabilities

### Build and deploy

- **Install, compile, and clean** from one panel — progress, cancellation, and elapsed time where you need them.
- **Gradle sync** via **Android: Sync Gradle project** from the panel, editor toolbar, or Command Palette.
- **Cached APK builds** — successful install / compile flows can copy APKs into a workspace cache. Open **Android: Open cached APK builds** for history with timestamps and duration metadata.

### Variants and modules

- Pick **Gradle root**, **module**, and **build variant** (**Android: Build Variants**) with searchable quick picks; selection is paused during active builds for consistent state.
- After **Android: Discover variants (Gradle)**, variant names are cached per module with debounced validation (no surprise Gradle runs in the background). While discovery runs, the panel shows a **circular progress** indicator on the discover action and in the busy row.

### Devices

- Target **physical devices** or **emulators** without memorizing `adb` flags.
- **Auto-selection** polls `adb` and picks a sensible default; if the current device disconnects, the extension falls back to the most recently active target.

### Console and logs

- **Logcat** — stream `adb logcat` in a webview with filters, color-coded levels, and auto-scroll. Open from the panel or **Android: Logcat**.
- **Build output with summary** — every run can end with a structured **BUILD SUMMARY**: result, duration, APK path, and extracted failure reasons.
- **Full build errors** — use **View Full Errors** on the notification or **Android: Show Full Build Errors** for a read-only tab with compilation errors and Gradle details in context.

### Debug (experimental)

- **Android: Attach Debugger to Process** lists processes, forwards JDWP over `adb`, and starts an **Android JDWP Debugger** attach session; **Android: Detach Debugger** ends it. The panel exposes attach / disconnect and shortcuts to VS Code stepping and breakpoints for **Java** and **Kotlin**. Behavior is still maturing — see **[CHANGELOG.md](CHANGELOG.md)**.

### Polish

- **JVM CPU** (bottom of the Android panel): polls local JVMs with **`jps`**, shows PID, CPU % (from `ps` on macOS/Linux or Windows perf counters), and a rolling **total CPU** sparkline with layout tuned for the webview. Uses the same JDK resolution as Gradle (**JDK for Gradle** / `JAVA_HOME`); install a JDK if you see “jps not found”.
- Optional **status bar** device label, **editor title** actions, and **open app after install** via `MAIN/LAUNCHER` (with **monkey** fallback). Application id resolution follows **applicationId** / **namespace** from Gradle, including **suffix** rules when applicable.
- Shortcuts from the editor keep the Gradle webview aligned with your latest module, variant, and device choices.

---

## Get started

1. Open the folder that contains your **Gradle wrapper** (`gradlew` / `gradlew.bat`).
2. Open the **Android** side panel — activity bar icon, or **Android: Open side panel** (**⇧⌘P** / **Ctrl+Shift+P**).
3. Choose a **device**, then **Install**, **Compile**, **Clean**, or **Sync Gradle** as needed.
4. Open **Logcat** from the panel or run **Android: Logcat**.
5. Optional: **Android: Open cached APK builds** after a successful flow to inspect cached artifacts and timings.

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
- **JAVA_HOME** and optional **isolated `GRADLE_USER_HOME`** for Gradle (panel **Settings** gear); **stop daemons** and **idle `--stop`** timing so extension Gradle does not fight Studio by default
- **extra Gradle arguments** for every invocation
- **status bar** device visibility and **launch after install** (plus optional **application id** override)

Sensible defaults work for typical **debug** workflows — no configuration required to start.

---

## Changelog

See **[CHANGELOG.md](CHANGELOG.md)** for release notes.
