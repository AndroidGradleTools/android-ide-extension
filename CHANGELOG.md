# Changelog

Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Newest first.

## [Unreleased]

## [1.4.0] - 2026-11-20

A reliability release that fixes every finding of a whole-codebase audit of 1.3.1.

- **Release signing.** Set up release signing now puts `signingConfig` inside `buildTypes { release { } }` of Groovy and Kotlin scripts instead of inside the new `signingConfigs` block, which broke the script. All three storage options now sign panel builds: the extension passes the values to every build it starts. Values in `~/.gradle/gradle.properties` are saved under project-scoped names, and you are asked before values saved for another key store are replaced. Passwords are checked against the key store before anything is saved and reach `keytool` through the environment, not its command line. New key store with Save & configure now stores the whole configuration. Blocks written by earlier versions keep working.
- **Flavors and modules.** Build Variants lists real variants only, not AGP's aggregate tasks such as `assembleDebug`. Switching module picks a variant that module has, a module picked before Sync is kept, and `android-ide-extension.defaultGradleModule` is used until you pick a module. An explicit Sync always refreshes the variant cache, which now also watches `buildSrc`, `build-logic` and applied scripts. Compose Preview uses the same checked cache.
- **The app you mean.** Logcat's default package filter, App Functions Lab, the agent context bundle and the agent tools resolve the applicationId of the variant you run, like the launch after install. Launch tries only the installed package when its id came from the build output.
- **Settings fields.** Text fields on the Settings page no longer reset while you type. Leaving a field writes only a changed value, and an emptied field clears the setting.
- **Windows.** Gradle runs work when the project path has spaces, `"` and line breaks in Gradle arguments are refused, and Stop ends the whole process tree. `sdk.dir` with escaped `\:` in `local.properties` is read correctly, quoted `-D` arguments stay whole, and the VSIX installer quotes an IDE path with spaces.
- **Debugger.** Breakpoints set before attaching are installed. They bind in top-level functions, companions, inner classes and lambdas, and files are matched by package. Objects expand in Variables. Class loads no longer pause every thread, `android-jdwp` launch configurations work without running Attach first, and broken connections end instead of hanging.
- **Logcat.** Log tags are no longer offered as packages, and package names are checked before any `adb shell` call. The package filter learns the app's process from the log, so crashes right after launch or Restart stay visible, and picking a package starts filtering at once. The device label stays current, the device picker lists running devices only, large buffers stay fast, and multi-byte text is no longer split.
- **Devices.** Your device pick is kept while the device is unplugged and restored when it returns. Run on an AVD installs only on that AVD and never starts a second emulator for it. A Gradle failure ends the boot wait at once. Installing an older cached APK works (`-d`). An AVD shows Connected only when it runs.
- **AI agents.** `android_check` refuses tasks that drive devices or rewrite source, including Gradle's abbreviated names, and its builds can't reach a device. Device actions are checked again after waiting for Gradle and before an install or launch. The context bundle redacts more kinds of secrets, results name the Gradle root, `android_run` reports the id it launched, and the logcat digest matches whole package ids.
- **Crash reports.** The sanitizer removes your home folder and user name first, including user names with spaces and any letter case on Windows. The `crashReportEnabled` description now says plainly that the report page address carries the sanitized report.
- **Remote ADB.** Stop works during connect, a second Start no longer leaves the first session running, large transfers no longer stall, and frames are capped in size. The relay APK download uses a private temporary file and checks the file it installs.
- **Gradle and files.** Stored build errors are cleared when a new panel job starts, the idle daemon stop re-checks for running builds, and stopping daemons no longer blocks VS Code. Build files and properties keep their line endings and are written in one step.
- **What's New.** A new What's New page covers this release, and its footer shows the installed version.
- **Release metadata.** Bumped the package, lockfile, feature catalog, and README release notes to 1.4.0.

## [1.3.1] - 2026-11-20

- **Build and Install launches the variant it installed.** With several flavors built, auto-launch could start another flavor's app because it read the newest APK in the module. It now takes the module and variant from Gradle's install log or the install task and reads that variant's `output-metadata.json`, even when packaging is up to date. When the variant can't be told apart, it says why in Build output and skips the launch. The APK cache now copies the same variant's APK, or says in Build output why it didn't.
- **Release metadata.** Bumped the package, lockfile, feature catalog, and README release notes to 1.3.1.

## [1.3.0] - 2026-09-30

- **What's New.** After an update, the Android panel shows a one-time "What's new in 1.3" tip. It opens a What's New tab with key highlights, illustrated how-to steps, and Try it actions for the new features. Reopen it any time with **Android: What's New** or ⋮ More → What's New.
- **Agent status in the panel.** While the agent MCP server is on, a chip in the panel's status strip shows the connected agent (or the number of agents) and the tool call in progress. Click it to see the endpoint, allow or disallow device actions, check each agent's last activity and recent tool calls, and connect, stop, or turn off. Only agent names, versions, and tool names are kept, never arguments or results. Agents that end their session (`DELETE /mcp`) leave the list right away.
- **Compose Preview on a device.** A **Preview on device** CodeLens above each `@Preview` function (and **Android: Run Compose Preview on Device**) installs the app module's debug variant on the selected device, opens the preview through `PreviewActivity`, and shows a screenshot beside the editor. It handles `@file:JvmName` and multifile facades, `@JvmStatic` members of objects and companions, built-in and same-file multipreviews, and `@PreviewParameter` providers. Test and non-Android source sets are marked unavailable with a reason. Diagnostics check the app module for `ui-tooling`. Turn the lens off with `android-ide-extension.composePreviewCodeLens`.
- **Agent MCP server (opt-in).** **Android: Connect AI Agent (MCP)…** starts a local Streamable HTTP server on `127.0.0.1:47831` (`agentMcpPort`) and copies configuration for Claude Code, Codex, or Cursor; VS Code registers it automatically when its MCP API is available. Tools: `android_state`, `android_check` (compiles only modules with changed files and returns structured Kotlin, Java, and AAPT errors), `android_run` and `android_preview` (both require `agentMcpAllowDeviceActions`), and `android_logcat` (app logs since last launch, crashes first). Requests need a bearer token from SecretStorage (**Android: Rotate Agent MCP Token**). The three settings apply only from user settings, so a workspace cannot enable them.
- **One Gradle queue.** Panel builds, syncs, previews, and agent runs now wait for each other instead of starting overlapping Gradle processes. Agent runs add to Build output under a separator and leave your last build's errors intact. Stop Gradle Task also cancels queued and agent runs.
- **Terminals use the extension's JDK.** New integrated terminals in an Android workspace get the extension's `JAVA_HOME` and matching `PATH` entry (`android-ide-extension.terminalGradleEnv`: `off`, `javaHome` by default, or `javaHomeAndUserHome` to share the Gradle home too). In multi-root workspaces only Android folders are affected.
- **Idle daemon stop spares running builds.** Idle auto-stop now checks `gradlew --status` and skips a Gradle root while any daemon is busy, since `gradle --stop` also ends running builds. Stopping a build only falls back to `--stop` when Gradle has not exited after a few seconds.
- **Sync cancel.** Cancelling Sync no longer caches an empty variant list for the module.
- **Build triage skill.** The bundled `android-build-triage` skill now guides agents to compile only the changed modules and run targeted unit tests.
- **Release metadata.** Bumped the package, lockfile, feature catalog, and README release notes to 1.3.0.

## [1.2.0] - 2026-09-28

- **Android Studio–style panel.** The Android panel now follows Android Studio's layout: a raised run widget (module | variant, deployment target, a green ▶ Run 'app', then Build, Sync, Attach Debugger and More with coloured icons; menus stay closed while Gradle runs), tool windows docked at the bottom with live details (Logcat, Build output and System log, then Other tools: APK builds, Signing, AppFunctions Lab and AI Toolchain), and a status strip with the Gradle state and a JVM meter. Notices appear only when something needs action. The healthy-state banner, section cards, tile grid, footer and CPU/RAM dock are gone.
- **Sync finds your variants.** Sync now refreshes the modules and build variants with the same Gradle commands and cache Discover Variants used, and it runs once by itself the first time a project opens (`android-ide-extension.syncOnFirstOpen`, on by default). A project with flavors gets a real variant, such as demoDebug, instead of an ambiguous `installDebug`. Clicking the variant with nothing cached syncs and opens Build Variants without a dialog. Before any Sync the variant reads "debug · default", and a failed Sync shows one notice with Retry. Discover Variants left the ⋮ menu and the Command Palette, and Clear Variant is now Use Default Variant.
- **Release signing notice: Don't show again.** Projects that sign release builds themselves can turn the release signing checks off for that project. Undo is one click away, and the Release signing page can turn them back on.
- **Settings that can't be saved say why.** If VS Code refuses a change from the Settings page or the keymap tip, you see the reason (with Reload Window when the window needs a reload after an update) instead of a value that silently reverts.
- **Stop and Stop Gradle Task.** While a build or install runs, Run turns into Stop with a timer. The new **Android: Stop Gradle Task** command cancels the running panel job.
- **Pages instead of sheets.** Settings, Release signing, Pair Devices Using Wi‑Fi, AppFunctions Lab and the new AI Toolchain page open as full-panel pages with Back, breadcrumbs and Esc, instead of sheets over a dimmed panel.
- **Studio-style Settings.** Settings are grouped like Android Studio's (Appearance & Behavior; Build, Execution, Deployment; Languages & Frameworks; Tools), with search, value summaries, modified markers and per-setting Reset. Changes still apply immediately.
- **Release signing labels.** The signing page uses Android Studio's New Key Store labels, a collapsible Gradle changes preview and a footer with Save & configure.
- **AI Toolchain page.** The Android CLI, official skills and an agent-by-skill grid for the extension skills have their own page. Agents without a skills folder stay visible, dimmed.
- **Optional Android Studio keymap.** Set `android-ide-extension.keymap` to `androidStudio`, or accept the one-time tip that lists the keys, for ⌃R Run, ⌘F9 Build, ⌘F2 Stop and ⌘6 Logcat (Shift+F10, Ctrl+F9, Ctrl+F2 and Alt+6 on Windows and Linux). Stop only takes ⌘F2 / Ctrl+F2 while a Gradle job runs.
- **Debugger controls.** The panel's step, pause, breakpoint, mute and evaluate buttons are removed; VS Code's debug toolbar and Run and Debug view provide them. The panel keeps a "Debugger attached" row with Detach.
- **View title.** Only Settings stays inline in the Gradle view title. The other actions moved to its ⋯ menu, and Sync lives in the run widget and the Command Palette.
- **Device picker wording.** The picker groups Running devices and Virtual devices and ends with Pair Devices Using Wi‑Fi….
- **Release metadata.** Bumped the package, lockfile, feature catalog, README badge, and release notes to 1.2.0.

## [1.1.1] - 2026-09-18

- **AppFunctions Lab layout.** Restored the full-panel inspector's opaque background so the normal Gradle controls, AI Agents banner, and JVM telemetry no longer show through or overlap its content.
- **Regression coverage.** Added a guard that keeps the Lab overlay connected to a defined shared panel background token.
- **Release metadata.** Bumped the package, lockfile, feature catalog, README badge, and release notes to 1.1.1.

## [1.1.0] - 2026-09-18

- **Structured AppFunctions metadata.** The experimental Lab now presents selected-app function identifiers, descriptions, parameters, referenced component schemas, and response types from real Android 16 `list-app-functions` output while retaining bounded raw JSON as the fallback and authority.
- **Standalone Android tools.** The main panel now remains useful without a Gradle project: device selection, wireless ADB, Logcat, system logs, settings, and AI Toolchain stay available while project-only controls are visibly disabled with a direct open/add-folder action.
- **Negative Logcat tag filters.** Prefix tag chips with `-` to exclude noisy tags, including mixed include/exclude filters and preset round-tripping.
- **Release metadata.** Bumped the package, lockfile, feature catalog, README badge, and release notes to 1.1.0.

## [1.0.1] - 2026-08-28

- **AppFunctions Lab foundation.** Added a compact **Lab** action to the existing Build toolbar and **Android: Inspect AppFunctions (Experimental)**. Both open an auto-running, full-panel inspector that keeps readiness checks, selected-device recovery, and registered metadata inline instead of opening a Markdown report or occupying permanent card space.
- **Read-only rollout.** The first lab slice never builds, installs, launches an AVD, changes function state, or executes a function. Execution remains deferred until descriptions, JSON parameters, and destructive-action confirmations can be reviewed explicitly.
- **Bounded device metadata.** Re-applied the 1 MiB limit after JSON pretty formatting so compact device output cannot expand into an oversized panel payload.
- **Reliable release packaging.** Excluded nested `.worktrees/` directories from VSIX contents and added a regression assertion for the publisher ignore rule.
- **Release metadata.** Bumped the package, lockfile, feature catalog, README badge, and release notes to 1.0.1.

## [1.0.0] - 2026-08-28

- **AI Toolchain.** Renamed the AI settings surface and added optional Android CLI status, official skill list/search, documentation, and explicit initialize/update actions. Skill mutations require the exact modal confirmation; settings load, probe, list, and search remain read-only.
- **Diagnostics JSON.** Added **Android: Export Diagnostics as JSON** with a stable schema-v1 document, Android CLI readiness, and local/Remote-SSH extension-host context while preserving the existing text report.
- **Agent Context Bundle.** Added **Android: Create Agent Context Bundle** as a lazy-loaded workflow that writes diagnostics, focused build errors, bounded Logcat, an explicit selected-device screenshot, and structured UIAutomator layout evidence to extension storage, then opens `context.md` for review.
- **Partial-failure evidence.** Every optional bundle artifact records captured, skipped, or failed state, so missing Android CLI, no selected device, stopped/unauthorized devices, and individual collector failures do not prevent the required manifest and review document.
- **Privacy and explicit action.** Added deterministic redaction for common secret assignments/query values, UTF-8-safe text limits, image/hierarchy/total size caps, unique UIAutomator paths with cleanup, no automatic clipboard/upload/model action, and user-selected copy/reveal actions only.
- **Remote-workspace clarity.** Declared workspace extension placement, rejected virtual workspaces that cannot provide Android tooling access, and exposed execution location in diagnostics and bundles.
- **Conservative Android CLI device capture.** The adapter supports `ANDROID_SERIAL`-scoped layout/annotated capture, but those optional artifacts remain disabled until two-device targeting is manually verified; trusted `screen.png` always uses explicit `adb -s <selected-serial>`.
- **Marketplace discovery.** Updated the display name to **Android Gradle Tools — Build, ADB, Logcat & AI**, retained build/install discovery terms, replaced generic duplicate tags with Android CLI and agent keywords, moved to the accurate Debuggers/Other categories, and added direct installation, search-intent FAQ content, and a non-interruptive review/support link to the README.
- **Release metadata.** Promoted the extension to 1.0.0, removed the public-beta README treatment, and aligned the package, lockfile, feature catalog, README badge, and Marketplace-facing copy.

## [0.3.1] - 2026-06-17

- **Stable AI skill links.** Bundled Android skills are copied into extension-managed storage and agent home folders now symlink to that stable location instead of the versioned extension package directory.
- **Extension-update migration.** Legacy extension-owned skill symlinks that point at old package resources are migrated automatically, while ordinary files/directories and unknown symlink targets remain protected.
- **ADB-only screen inspection.** `android-screen-inspector` no longer ships or requires a Node helper script; its workflow now documents direct `adb` screenshot and UIAutomator hierarchy commands.
- **Regression coverage.** Added guards for stable skill links across extension updates and for keeping the screen-inspector skill free of Node / `.cjs` script dependencies.
- **Release metadata.** Bumped package metadata to 0.3.1 and aligned README release notes and feature catalog metadata.

## [0.3.0] - 2026-06-13

- **Android diagnostics preflight.** Added **Android: Run Diagnostics**, a read-only command that writes a one-shot setup report to the Android System output channel.
- **Setup report coverage.** Diagnostics now summarize detected Gradle roots, active root, Gradle wrapper state, active module / variant, cached variants, SDK root, adb / emulator / aapt resolution, JAVA_HOME, GRADLE_USER_HOME, selected target, ADB devices, and AVDs.
- **Command surfacing.** Exposed the diagnostics command through activation events, the Command Palette, and the Android panel title actions next to the System Gradle Log.
- **Read-only guardrails.** The diagnostics service avoids Gradle execution, APK install paths, file writes, and directory creation while still reporting missing or deferred environment state.
- **Regression coverage.** Added guards for diagnostics command wiring, deterministic report formatting, output-channel usage, and read-only service shape.
- **Release metadata.** Bumped package metadata to 0.3.0 and aligned README release notes and feature catalog metadata.

## [0.2.12] - 2026-06-12

- **Logcat safety and control.** Opening or restarting Logcat no longer clears the device-wide log buffer by default. A separate **Clear device buffer...** action in the overflow menu now confirms before running `adb logcat -c`, while the toolbar clear button remains local to the viewer.
- **ADB reliability.** Key ADB and tool subprocess paths now use explicit timeouts and cancellation handling so bad device / server states are less likely to leave extension flows hanging indefinitely.
- **Variant discovery caching.** Gradle assemble variant discovery is fingerprinted per module and reused when relevant Gradle files are unchanged. Discovery cache also survives reopening the same project under a different VS Code workspace identity, while the selected variant stays workspace-local.
- **Performance fixes.** APK cache copies now stream through `workspace.fs.copy()` instead of buffering entire APK payloads in extension-host memory. JDWP source lookup avoids synchronous recursive workspace scans and uses bounded, cached async search.
- **Background work gating.** Device polling now starts only after an Android Gradle project context is confirmed, reducing startup work in unrelated workspaces.
- **Regression coverage.** Added guards for non-destructive Logcat startup, explicit device-buffer clearing, exec timeout / cancellation behavior, APK copy memory usage, variant cache reuse, cross-workspace variant-cache restore, async debug source lookup, and Android-only device polling.

## [0.2.11] - 2026-06-08

- **Unified panel visual language.** Brought the Android side panel, panel settings, Wireless Remote ADB guide, Logcat toolbar / menus, APK cache menus, inputs, popovers, and active states onto shared Material 3 / JetBrains-style surface tokens.
- **AI Agents symlink replacement.** Broken or third-party symlinks can now be replaced only after an explicit confirmation, while real user-owned files are still preserved.
- **Quieter AI Agents banner.** The banner now hides once every detected agent has all bundled Android skills linked, and panel state refreshes immediately after skill-link changes.
- **UI / UX contract.** Expanded the project rules with the Android panel visual-language contract so future panel, toolbar, menu, row, empty-state, and popover work has one source of truth.
- **Release metadata.** Kept the release manifest identity, README badge, lockfile, and feature catalog aligned for 0.2.11.
- **Regression coverage.** Added guards for all-detected-agent skill linking, symlink replacement behavior, panel-state refresh, and the updated AI Agents settings markup.

## [0.2.10] - 2026-06-08

- **Bundled Android AI skills.** Added `android-build-triage`, `android-logcat-debugger`, and `android-device-lab` alongside the existing read-only `android-screen-inspector` skill.
- **Broader AI Agents support.** The panel can link bundled skills into detected Codex, Claude, Copilot, Gemini, and Cursor home folders with extension-owned symlinks.
- **AI Agents discovery banner.** Added a banner above the CPU / RAM dock that opens the AI Agents settings section, can collapse, and stays dismissed for 7 days unless a new agent folder appears.
- **Cleaner AI Agents settings.** Reworked provider toggles into compact chips, hides unavailable providers, separates each skill into a readable section, and shows only actionable per-skill issue messages.
- **Release docs alignment.** Updated README release notes and feature catalog metadata for the 0.2.10 release.

## [0.2.9] - 2026-06-07

- **Marketplace discovery metadata.** Refined the manifest description and keyword set for Android search intent while keeping the public name aligned with the `androidgradletools.org` domain and `Android Gradle Tools` brand.
- **Android search coverage.** Added focused Android terms for Gradle, ADB, Logcat, APK, emulator, deploy, and debugger workflows within the Marketplace 30-keyword limit.
- **Release docs alignment.** Updated README release notes, version badge, and feature catalog metadata for the 0.2.9 release.

## [0.2.8] - 2026-06-07

- **Marketplace search pass.** Updated the extension description to lead with Android / VS Code / Cursor intent while keeping the `Android Gradle Tools` brand aligned with `androidgradletools.org`.
- **Keyword coverage.** Filled the Marketplace keyword list to the 30-keyword limit with focused Android terms for Gradle, ADB, Logcat, APK install, emulator, deploy, and debugger workflows.
- **Feature catalog sync.** Mirrored the marketplace-facing description in `features.yaml` so landing page and README generation inputs stay aligned.

## [0.2.7] - 2026-06-07

- **AI agent screen inspector skill.** Added a bundled read-only `android-screen-inspector` skill with an adb-first script that captures screenshots and UIAutomator hierarchy data without running a local server.
- **AI Agents settings.** Added a skill-first **AI Agents** settings section where each skill lists provider toggles for Claude and Codex. Enabling a provider creates an extension-owned symlink into the detected agent home; disabling removes only that symlink and preserves user files/conflicts.
- **MCP removal.** Removed the MCP server contribution, settings, auto-registration flow, source modules, bundle entry, and dependency in favor of the standalone skill path.
- **JVM CPU / RAM dock.** Expanded the panel telemetry dock with resident memory totals, per-process RAM chips, and separate CPU / RAM sparklines while keeping the compact hide/show behavior.
- **Loading and empty-state polish.** Reworked the initial loading skeleton with a smoother transform-based shimmer and replaced the non-Gradle workspace fallback with a centered icon + text empty state.
- **Release metadata.** Bumped package metadata to 0.2.7.
- **Regression coverage.** Added guards for the AI agent skill linker, MCP removal, RAM telemetry dock, transform-based shimmer, and non-Gradle empty-state markup.

## [0.2.6] - 2026-06-07

- **Panel cleanup.** Removed the optional mascot/activity feature, including its panel dock, animation assets, editor/terminal activity listeners, local beacons, and AI input hook installer.
- **Settings cleanup.** Removed the matching configuration keys from the extension manifest and feature catalog.
- **Regression coverage.** Added a guard that verifies the removed feature no longer appears in the shipped source surfaces.

## [0.2.5] - 2026-06-07

- **Panel visual pass.** Refined theme-dependent panel rendering and repaint behavior.

## [0.2.4] - 2026-06-07

- **Android panel visual polish.** Refined the main panel for a more modern minimal look in light and dark themes: softer section surfaces, lower-contrast borders, fixed-size Build actions, and a darker settings sheet that stays readable above the panel.
- **Gradle command UX.** Busy rows now show cleaner resolved Gradle labels, keep elapsed time aligned near **Cancel**, optimistically disable running actions, and treat Gradle build failures as user-facing errors instead of extension crashes. Cancel also stops the matching Gradle daemons.
- **JVM CPU dock polish.** Reworked the CPU strip into a compact telemetry section with process count, total CPU, lighter chips, a sparkline, and a hide/show control. When hidden, polling stops until the dock is shown again.
- **Regression coverage.** Added tests covering the refreshed panel surface, Gradle busy state, CPU telemetry dock, and safer Gradle runner behavior.

## [0.2.3] - 2026-06-07

- **README polish.** Reworked the README into a tighter hybrid of Marketplace pitch and technical reference: clearer hero copy, grouped workflow sections, command / settings tables, and less repeated feature text.
- **Easier setup messaging.** Highlighted the visual panel settings sheet as the main configuration path for JDK, Android SDK, Gradle daemon cleanup, MCP, Remote ADB, and panel polish.
- **Release metadata.** Bumped package metadata to 0.2.3 and kept the feature catalog aligned with the release version.

## [0.2.2] - 2026-06-06

- **Logcat crash retention.** Package-filtered Logcat sessions now keep historical PIDs for the selected package, so logs captured before a crash, ANR, or force-stop remain visible after the app process exits.
- **Logcat export.** Export now considers those historical package PIDs too, preventing post-mortem logs from disappearing when `adb shell pidof <package>` returns empty.
- **Regression coverage.** Added a test for the crash-path filter case where the active PID set becomes empty but buffered entries from the previous package PID should still match.

## [0.2.0] - 2026-06-06

- **MCP server for AI assistants.** Local Streamable HTTP server exposes `android_screenshot` and `android_view_hierarchy` against the panel-selected device. The panel keeps the endpoint visible when enabled; older VS Code / Cursor hosts can still use the URL directly.
- **VS Code native MCP discovery.** Added the required `contributes.mcpServerDefinitionProviders` manifest entry and fixed provider ids for stable vs debug builds. Native discovery now resolves the HTTP definition only after the listener is ready, preventing `fetch failed` races on `127.0.0.1:<port>/mcp`.
- **MCP auto-registration.** Workspace `.mcp.json` sync now removes stale `android-gradle-tools` entries when MCP is disabled or auto-register is switched off, preserves other servers, and logs write/remove actions to the System output channel.
- **MCP protocol handling.** Streamable HTTP requests now use a fresh stateless transport per request, so SDK clients can initialize and list tools reliably without sharing a stale transport.
- **Quality guardrails.** Added tests for the MCP HTTP server, native provider resolution, manifest contribution, UI settings behavior without native discovery, and `.mcp.json` cleanup.

## [0.1.35] - 2026-05-07

- **Logcat — adjustable font size.** Overflow menu (⋮) gains a **Font size** row: 11-step scale seekbar (0.5× → 2×) with Scale / Fixed toggle and reset (↺). Persisted across restarts. Layout columns (timestamp, PID, tag) are `em`-based so they scale proportionally.
- **Panel settings — Logcat section.** New **Logcat** group below **Panel UI** exposes Timestamp format and font size (synced bidirectionally with the overflow menu).
- **Bug fixes.** Font double-apply on prefs echo-back; `onPrefsChanged` subscription accumulation on sidebar re-resolve; CMD+/- double-scale caused by browser-level zoom firing inside webviews.

## [0.1.34] - 2026-04-26

- **Build card — module picker locked during Install/Compile.** While Gradle is busy, the module breadcrumb is disabled (matching the variant picker). The device pill stays clickable so you can still re-target without canceling the build.
- **Settings overlay — refreshed group icons.** Each group now uses a glyph distinct from the rest of the panel: **JDK & Gradle home** (coffee), **Build & install** (rocket), **SDK & emulator** (Android robot), **Panel UI** (split-panel dashboard), **Diagnostics** (troubleshoot). Removes overlap with the Compile / device-chip / debugger icons used elsewhere in the panel.
- **Internet relay — Protocol button opens in-editor.** "Protocol" now opens `docs/remote-adb-bridge-protocol.md` as a Markdown preview tab inside VS Code (previously launched the external default app for `.md`).

## [0.1.33] - 2026-04-26

- **Repository moved to the [AndroidGradleTools](https://github.com/AndroidGradleTools) GitHub organization.** Source, screenshots, the in-panel **Wireless** guide links, and the in-extension `adb-relay-android` APK download URL all point at the new org. Sibling projects ([adb-relay-jvm](https://github.com/AndroidGradleTools/adb-relay-jvm), [adb-relay-android](https://github.com/AndroidGradleTools/adb-relay-android)) moved together. No code or behavior changes — pure org migration.

## [0.1.32] - 2026-04-26

- **Webview assets minified via esbuild:** moved JS/CSS source to `webview/`; `media/*.{js,css}` are now build artifacts (gitignored). Total payload **218.7 KB → 138.8 KB** (~37% smaller). Build hooks: `build:media`, `watch:media`, plus auto-run on `compile`/`vscode:prepublish`/`postinstall`.

## [0.1.31] - 2026-04-26

- **Settings overlay:** master-detail layout with icon'd group list (Android-17 style). All ~20 extension configs editable in-panel — was 3. Per-row reset (↺) to package.json default; bottom links to Wireless guide + native VS Code Settings.
- **Wireless header:** chevron back + left-aligned title (matches Settings detail). Entered from Settings → back returns there, **×** hidden.
- **Device pill — live status dot** (mirrors Logcat): green = adb-online, red = offline/unauthorized, amber = AVD not yet running, muted = none. Cached `adb devices` (1.5s TTL); panel re-renders on device set change so the dot stays current after plug/unplug.

## [0.1.30] - 2026-04-26

- **Crash reporter:** when our code throws an unexpected error, a notification offers **Report** / **Show log** / **Dismiss**. **Report** opens a webpage with the sanitized payload pre-filled — nothing leaves your machine until you press Submit. Disable with `android-ide-extension.crashReportEnabled = false`.
- **Android Errors** output channel logs every caught error regardless of whether a notification was shown.
- **Bug-only filter:** Gradle, ADB, missing-device, and config failures never trigger a crash report — only errors classified as extension bugs do.
- **Bundled VSIX** via esbuild multi-entry with lazy chunks (signing, debug attach, remote ADB): **175 → 24 files**, **318 KB → 205 KB**.

## [0.1.29] - 2026-04-25

- **Sign chip** in the Build card header — switch keystore per build. **Default** = Gradle's own signing; saved signatures inject `-Pandroid.injected.signing.*` at build time.
- **Setup `<Variant>` signature** overlay: pick an existing keystore (Detect aliases) or **Generate new** via `keytool`. Passwords go to OS keychain.
- **Auto-inject `signingConfigs`** into `build.gradle.kts` or `build.gradle` (Groovy DSL) with diff preview; idempotent.
- **Logcat — perf:** incremental tag-dedup (was scanning all ~8 000 rows per batch); fixed PID-resolve re-entrancy / stale-result race on rapid filter/device switches.
- **Polish:** per-tab `aria-busy` / `aria-disabled` on project switcher; red `border-left` accent on `java-procs` error chip; POSIX single-quote escaping for Gradle args (`$` / backtick safe).

## [0.1.28] - 2026-04-25

- **Logcat:** new **Crash** and **Stack trace** entries in the Level dropdown for quick narrowing; an **All levels** toggle at the top.
- **Logcat — Timeline:** consecutive rows sharing the same tag hide the repeat for cleaner reading.
- **Logcat — selection bar** is now a floating overlay; no longer pushes log output down.
- **Logcat — UX polish:** inline regex feedback, tighter filter responsiveness, scroll-paused indicator, preset overwrite confirm, tag-chip *Create* suggestion, **Esc** focus return, trash icon for **Clear**.
- **Build & install:** improved cancel and lifecycle handling around AVD cold-boot, terminal-task builds, and variant validation.
- **APK cache:** new **Android: Clean APK Cache** command; the cache panel preserves scroll position on refresh.
- **Debug:** more reliable cleanup when an attach session disconnects unexpectedly.
- **Build output:** clearer diagnostics when `applicationId` cannot be resolved automatically.

## [0.1.27] - 2026-04-25

- **Unified UI style:** checkboxes, radios, and primary buttons across all panels redrawn in **Material 3 Expressive** with JB-style accent.
- **Toggle color:** **Timeline**, **Soft-wrap**, and Search toggles now use the same primary blue as **Install APK** when active.

## [0.1.26] - 2026-04-25

- **Panel — unified Build card:** *Build flavor* and *Device & build* merged into one **Build** card. The `module / variant` breadcrumb and the device chip are now inline pickers — click `module`, `variant`, or the device pill to open the matching picker. **Discover variants** and **Clear variant** moved into a `⋮` overflow menu.
- **Variant pick no longer overwrites the module:** the variant quick-pick is scoped to the **current module** and only writes the variant back. If that module has no cached variants, you're prompted to run **Discover variants** for it.
- **Logs section icons:** **smartphone** (Logcat), **wrench** (Build output), **article** (System log) — replaces the previous terminal / article / sliders set.
- **Project tabs:** softer active-tab highlight (1px tinted border, no drop-shadow).

## [0.1.25] - 2026-04-25

- **Logcat — toolbar:** redesigned bar with in-toolbar **device picker** (status dot + chevron) and **package filter** dropdown; **Tags** and **Level** consolidated into chevron menus; **Timeline** toggle replaces the format/timestamp button.
- **Logcat — level filter:** multi-select **V / D / I / W / E / F** chips (replaces the single "minimum level" dropdown); badge shows count when not all are active.
- **Logcat — package filter:** accepts **comma-separated packages**; quick-pick combines **tags from logs** + **installed packages** (`pm list packages`) with multi-select and a *Clear selection* action; live **PID re-resolve every ~2s** so app restarts keep matching.
- **Logcat — project autodetect:** on open, package filter is pre-filled from the workspace's resolved Android `applicationId` (module + variant aware).
- **Logcat — stack traces:** click `File.kt:42` style links in messages to open the file at that line via `workspace.findFiles` (with disambiguation pick on multiple matches).
- **Logcat — export:** **Export** action saves the currently filtered entries to `.txt` / `.log` via the Save dialog.
- **Logcat — presets:** save / load / delete up to **10 named filter presets** (level + tag + package + query + regex), persisted in `globalState`.
- **Logcat — overflow menu:** *Collapse repeated rows* and other view options moved into a `⋮` overflow menu.

## [0.1.24] - 2026-04-23

- **Logcat:** tag chip input with autocomplete; **Formatted view** toggle (ms timestamp + second timeline separators); row selection (shift-click range), **Copy selected**, per-line copy button.
- **Debug:** **EXP** badge restored on Attach debug; `DebugAdapterDescriptorFactory` registered lazily on first attach.
- **Internet relay:** Install APK device picker restored; download shows live % progress.
- **Remote ADB bridge:** socket pauses on high WS buffer and resumes on `drain`.

## [0.1.23] - 2026-04-22

- **Wireless Remote ADB** re-enabled: panel tabs (Same Wi-Fi / Internet relay / VPN/mesh) are now visible by default.
- **Internet relay — Install APK:** device picker with **Refresh** + **Install APK** button; inline status feedback; downloads and installs `adb-relay-android` directly from the IDE.
- Removed bundled `tools/remote-adb-relay/relay.cjs`; relay host is now the standalone **[adb-relay-jvm](https://github.com/AndroidGradleTools/adb-relay-jvm)**.
- **Debug:** `DebugAdapterDescriptorFactory` is now registered lazily — only when the user first clicks **Attach debug** — fixing "can only be registered once" errors on extension reload.
- **Debug:** Removed **EXP** badge from the Attach debug toolbar button; tooltip still says "experimental".
- **Internet relay:** Removed the **Copy config** snippet and button from the ADB Relay step.

## [0.1.22] - 2026-04-21

- **Launch after install:** **`cmd package resolve-activity --brief`** → **`am start -n`** when a component is returned; else **`am start`** with **MAIN/LAUNCHER** + package, then **`monkey`**. Try several **installed package ids** (`pm list` / **`pm path`**, including **`.debug`**) until one works.
- **Launch package detection:** **`applicationId` from the installed APK** via **`aapt dump badging`** — path from the **Gradle “Installing APK …”** line when possible, else **newest `build/outputs/apk`** for the module; hints if **`aapt`** is missing; Gradle fallback includes **Kotlin DSL** **`applicationIdSuffix`** in **`getByName` / `named` / `create` / `register`** blocks.

## [0.1.21] - 2026-04-20

- **Remote ADB over WebSocket** (optional): dev-side mux bridge (**Android: Start / Stop Remote ADB Bridge**), settings `remoteAdbRelayUrl`, `remoteAdbSessionToken`, `remoteAdbListenHost`, `remoteAdbLocalPort`; protocol **docs/remote-adb-bridge-protocol.md**; **tools/remote-adb-relay/** relay + LAN agent; npm script **`remote-adb:relay`**. Depends on **`ws`**. **Panel + command + device picker**: tabbed guide (**Same Wi‑Fi** / **Internet relay** / **VPN / mesh**), **●** indicator + **resume tab**. **Internet relay**: compact steps, **ADB Proxy** + optional Node agent, **Save & start bridge** (writes settings, restarts bridge). Copy **for phone** (URL + token).
- **SDK / adb**: optional `androidSdkPath`, env, or `local.properties` → consistent `adb` / `emulator` (Logcat, debug, launch, installs). Cold AVD + panel **Install**: **assemble** overlaps emulator start; emulator logs → **System Gradle Log**.
- **Devices**: auto-select keeps an offline AVD choice; **Choose…** busy while listing; warning if AVD list fails.

## [0.1.20] - 2026-04-18

- JVM CPU: sparkline scale near 0%, canvas/chip layout, dock spacing and contrast.
- Discover busy: circular progress on button + busy row (replaces spinning search icon).

## [0.1.19] - 2026-04-18

- **JVM CPU** dock: `jps` list, CPU %, aggregate sparkline; polls while panel visible; `jps` from JDK for Gradle → `JAVA_HOME` → `PATH`.

## [0.1.18] - 2026-04-18

- Default **isolated `GRADLE_USER_HOME`**, optional custom path; **Stop Gradle daemons** + idle `--stop` after N minutes; settings for window close / toast / log.
- **Codicons** and editor title order; panel + **Logcat** use inline SVG icons.

## [0.1.17] - 2026-04-18

- Marketplace **categories**: Programming Languages, Debuggers (was Other).

## [0.1.16] - 2026-04-18

- New marketplace icon; generate scripts; high-res source not in VSIX.

## [0.1.15] - 2026-04-17

- Panel **Settings** (gear): pick **JDK for Gradle** — scans installed JDKs, updates **`android-ide-extension.gradleJavaHome`** (workspace or user). **Android: Panel settings (JDK for Gradle)** command.
- **MIT LICENSE** in the repository root.
- **Discover variants** failures: compact **Retry** row plus full details in **System Gradle Log** (no long inline error in the panel).
- **README** overhaul (overview hero, capability tables, GitHub-hosted **overview** screenshot); retired older multi-screenshot set.

## [0.1.14] - 2026-04-17

- Disable **Cache APK on Compile / Install** while Gradle runs or variants are discovering (same as toolbar); dimmed when disabled.

## [0.1.13] - 2026-04-17

- Minify `out/` with Terser on publish; `build-vsix.cjs` recompiles after packaging so local `out/` stays readable.

## [0.1.12] - 2026-04-17

- **Experimental:** Android JDWP attach debugger (`android-jdwp`), DAP over JDWP + adb forwarding, process picker, **Android: Attach / Detach Debugger**, panel Debug card, Java/Kotlin breakpoints; diagnostics in System Gradle Log. Experimental only.
- Variant validation uses the cached list from **Discover variants** only (no extra Gradle resolve); rerun discover if stale.
- Panel UI refresh; device picking consolidated in `deviceInstallSerial.ts`.

## [0.1.11] - 2026-04-16

- Build output ends with a **BUILD SUMMARY** block; failed builds get a stored error doc.
- **Android: Show Full Build Errors** opens full errors; failed-build notification adds **View Full Errors**.
- Marketplace `displayName` / description / `logcat` keyword update.

## [0.1.10] - 2026-04-16

- Logcat webview (filters, colors, auto-scroll) + **Android: Logcat**; **System Gradle Log** channel + command/button.
- Auto device selection when adb devices change.
- Output card → **Console & Logs** (Logcat, Build, System).

## [0.1.9] - 2026-04-15

- Panel busy row + Cancel while discovering variants; cancel shared with progress dialog.
- Fix duplicate `--console=plain` on discovery Gradle CLI.

## [0.1.8] - 2026-04-15

- Discovery Gradle runs stream to Build channel when enabled; discovery uses `gradleExtraArgs`.
- Shared `LineBuffer` for spawn runner and CLI discovery.

## [0.1.7] - 2026-04-15

- **Sync Gradle** command (title bar, panel, palette).
- Cached APK copies after compile/install + **Open cached APK builds** webview; panel preview.
- Multi-root helpers (`androidModuleKind`, `gradleRootLabels`).
- VSIX default path `.build/`; panel/workspace/variant UX; root/module pick locked during builds; post-install `am start` + applicationId suffix resolution; device/project store tweaks.
- Fix variant alignment with Gradle/module context.

## [0.1.6] - 2026-04-15

- Exclude `media/screenshots` from VSIX; README images from GitHub URLs.

## [0.1.5] - 2026-04-15

- README screenshots + `scripts/png-rounded-corners.py`.

## [0.1.4] - 2026-04-15

- More reliable Gradle subproject / variant discovery.
- Debounced stored-variant validation against Gradle output.

## [0.1.3] - 2026-04-14

- Publisher `AndroidGradleTools`, id `AndroidGradleTools.android-ide-extension`; icon, keywords, banner, description.
- Generic variant picker copy; packaging/publish script updates.

## [0.1.2] - 2026-04-14

- Initial baseline before marketplace identity changes.

[0.2.5]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.2.4...v0.2.5
[0.2.4]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.2.3...v0.2.4
[0.2.3]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.2.2...v0.2.3
[0.2.2]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.2.1...v0.2.2
[0.2.1]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.2.0...v0.2.1
[0.2.0]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.35...v0.2.0
[0.1.35]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.34...v0.1.35
[0.1.34]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.33...v0.1.34
[0.1.33]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.32...v0.1.33
[0.1.32]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.31...v0.1.32
[0.1.31]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.30...v0.1.31
[0.1.30]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.29...v0.1.30
[0.1.29]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.28...v0.1.29
[0.1.28]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.27...v0.1.28
[0.1.27]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.26...v0.1.27
[0.1.26]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.25...v0.1.26
[0.1.25]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.24...v0.1.25
[0.1.24]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.23...v0.1.24
[0.1.23]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.22...v0.1.23
[0.1.22]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.21...v0.1.22
[0.1.21]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.20...v0.1.21
[0.1.20]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.19...v0.1.20
[0.1.19]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.18...v0.1.19
[0.1.18]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.17...v0.1.18
[0.1.17]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.16...v0.1.17
[0.1.16]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.15...v0.1.16
[0.1.15]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.14...v0.1.15
[0.1.14]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.13...v0.1.14
[0.1.13]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.12...v0.1.13
[0.1.12]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.11...v0.1.12
[0.1.11]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.10...v0.1.11
[0.1.10]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.9...v0.1.10
[0.1.9]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.8...v0.1.9
[0.1.8]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.7...v0.1.8
[0.1.7]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.6...v0.1.7
[0.1.6]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.5...v0.1.6
[0.1.5]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.4...v0.1.5
[0.1.4]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.3...v0.1.4
[0.1.3]: https://github.com/AndroidGradleTools/android-ide-extension/compare/v0.1.2...v0.1.3
[0.1.2]: https://github.com/AndroidGradleTools/android-ide-extension/releases/tag/v0.1.2
