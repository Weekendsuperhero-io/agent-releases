# Changelog

All notable changes to Agent are documented here. Each release section can be used directly as release notes.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Versions follow [Semantic Versioning](https://semver.org/).

---







## [Unreleased]

## [0.3.14] — 2026-04-08

### Added

- **Runtime tracking** — added a `runtimes` database table to definitively track installed Node.js and uv paths, using `walkdir` to discover exact executable locations post-download.
- **In-app release notes** — added a UI section to display patch notes directly in the settings panel when an application update is available.

### Changed

- **ACP runtime resolution** — updated runtime path injection for registry-installed agents to securely resolve locations from the new `runtimes` database table instead of client metadata.
- **CI infrastructure** — migrated GitHub Actions workflows to use custom Warp runners and caching to improve build and test performance across all platforms.
- **Release pipelines** — updated the release pipeline to capture dynamically generated release notes and include them in the update payload.
- **Startup video loading** — simplified the intro video player to load assets directly via Vite instead of fetching and constructing blob URLs.
- **Automated signatures** — updated PR descriptions and changelog generation scripts to use current product branding.

## [0.3.13] — 2026-04-08

### Changed

- **ACP client state** — centralized configuration, connection probing, and session fetching into Zustand stores to simplify component architecture and state management.
- **MCP server state** — moved computation of installed registry metadata into the Zustand store to improve performance and code organization.
- **Agent runtime resolution** — migrated injection of bootstrapped registry paths to the Rust backend during capability probing and session fetching to ensure isolated runtimes are used.

### Fixed

- **NPX package installations** — fixed registry installation failures for scoped packages by properly stripping existing version suffixes before applying the pinned version.

## [0.3.12] — 2026-04-07

### Added

- **OAuth pre-registration** — added support for configuring pre-registered OAuth apps (e.g. GitHub) via a new Client ID field in MCP server settings.
- **Registry install progress** — added real-time progress bars to the registry browser during agent and server installations.
- **Secrets metadata tracking** — added a database table to track metadata, owner relationships, and expiration dates for all credentials stored in the OS keyring.
- **Combined logging** — introduced a unified `combined.log` file that captures all chronological logging output to simplify debugging.

### Changed

- **Keyring backend** — replaced `secret-lib` with `keyring-core` backed by platform-specific native stores (macOS Keychain, Windows Credential Manager, Linux Secret Service).
- **Keyring namespace** — updated the keyring service name to use the app bundle identifier, and added an automatic migration for legacy secrets on startup.
- **MCP settings UI** — redesigned the server list into a grid of cards with inline transport badges, update buttons, and clear OAuth authorization states.
- **Agent settings UI** — agent cards now display their configured default task instead of a generic description, and the entire card is clickable.
- **macOS runtime permissions** — quarantine xattrs on downloaded runtimes are now stripped in-process via `libc::removexattr` to prevent App Sandbox subprocess denials.
- **Unix shebang resolution** — scripts spawned on Unix now explicitly resolve their shebang interpreter against the injected PATH to ensure isolated runtimes are used.
- **Skill directory access** — security-scoped bookmark resolution for custom skill directories is now handled centrally in the backend during discovery.

### Fixed

- **OAuth 401 handling** — HTTP backends returning 401 Unauthorized now correctly prompt for OAuth authorization instead of recording generic connection failures.
- **Gateway synchronization** — the MCP gateway now automatically reloads backends immediately after a successful OAuth token exchange.
- **Redundant installations** — registry installations are now skipped if the requested version is already installed locally.
- **Registry errors** — registry browser error messages can now be manually dismissed and will automatically clear after 5 seconds.

## [0.3.11] — 2026-04-07

### Added

- **Remote MCP servers** — added registry support for installing and configuring remote MCP servers via HTTP/SSE transports.
- **Agent stderr capture** — agent child process stderr is now captured during initialization and injected into connection error messages for improved diagnostics.
- **Protocol message events** — JSON-RPC messages for initialization, session management, and authentication are now persisted as raw session events for auditing and replay.

### Changed

- **State management** — migrated frontend architecture from React Context to Zustand stores for themes, tabs, notifications, and settings to improve performance and code organization.
- **Windows CI performance** — expanded Windows Defender exclusions to cover Node.js, pnpm, and compiler processes for faster builds.
- **Release build profile** — changed link-time optimization (LTO) from full to thin to reduce compilation times.
- **Chat transcript retention** — updated the settings UI to provide a dropdown with common presets (10 days, 1 month, 1 year, forever) alongside custom day input.

### Fixed

- **Registry entry resolution** — entries not found in the local cache now trigger a fresh registry fetch or targeted search to correctly resolve remote-only servers.

## [0.3.10] — 2026-04-06

## [0.3.9] — 2026-04-05

### Added

- **Registry browser icons** — entry icons now display inline with agent/server names. Icons are resolved to data URIs at fetch time and cached. `dark:invert` applied for dark mode. Icon picker disabled for registry-installed agents.
- **View Logs button** — Settings → General now has a "View Logs" button that opens the app data directory in Finder/Explorer.
- **Spawn failure diagnostics** — cross-platform diagnostics when a child process fails to launch: file existence, permissions, PATH resolution, shebang/interpreter checks. On macOS: quarantine xattr, Gatekeeper assessment, code signature, and sandbox entitlement status.
- **Startup entitlements dump** — logs effective entitlements at startup via Security.framework for TestFlight debugging.
- **`ENTITLEMENTS.md`** — reference doc for both entitlement profiles.

### Changed

- **Structured logging** — removed `acp_log!`/`acp_debug!`/`acp_error!` macros (41 call sites), replaced with direct `tracing` calls using structured fields. Terminal lifecycle events promoted to `info`; agent stderr promoted to `warn` for release build visibility.
- **Project path opens directly** — clicking the project path pill in the session header now opens the directory (`openPath`) instead of revealing it in the parent folder.
- **unknownUpdate event type** — `event_type_str()` returns the actual type from raw JSON instead of hardcoded `"unknownUpdate"`.

### Fixed

- **Startup video interrupted** — intro video was cut short in release builds because `onComplete` callback changed on every render, causing effects to re-run. Stabilized via ref + `useCallback`.
- **Cmd+C / Ctrl+C copy** — keyboard copy wasn't reaching the WebView's native copy handler. Added a global keydown handler that writes plain text to clipboard via the Tauri clipboard plugin.
- **Registry agent icons lost on install** — `fetch_icon_data_uri` was trying to HTTP GET data URIs from cached registry entries, silently failing. Now passes through data URIs unchanged.
- **Registry agent runtime PATH** — `_extra_paths` from metadata wasn't being injected into spawn env vars. Fixed in backend (reads metadata from DB) and frontend (`buildAgentEnv` helper).

### Security

- **App Store entitlements** — added `files.user-selected.executable`, media folder access, microphone, Apple Events. Removed incorrect `inherit` and unnecessary hardened runtime exceptions.

---

## [0.3.8] — 2026-04-02

### Added

- **App Store sandbox support** — entitlements for sandboxed distribution, security-scoped bookmarks, and process spawning.
- **Cmd+C copy** — native keyboard copy support for WebView content.
- **Runtime PATH injection** — bootstrapped runtimes found via `_extra_paths` metadata.

### Fixed

- **Process spawning** — child process lifecycle, session cleanup, and spawn diagnostics.

---

## [0.3.7] — 2026-04-02

### Fixed

- Process spawning fixes and App Sandbox support.

---

## [0.3.6] — 2026-04-02

### Fixed

- Bug fixes and stability improvements.

---

## [0.3.4] — 2026-04-01

### Fixed

- Bug fixes and dependency updates.

---

## [0.3.3] — 2026-04-01

### Fixed

- Bug fixes and dependency updates.

---

## [0.3.1] — 2026-04-01

### Fixed

- Bug fixes and dependency updates.

---

## [0.3.0] — 2026-03-31

### Changed

- Internal improvements and dependency updates.

---

## [0.2.2] — 2026-03-31

### Changed

- Internal improvements and dependency updates.

---

## [0.2.1] — 2026-03-30

### Added

- **Registry browser** — browse and install ACP agents and MCP servers from official registries with distribution type badges.
- **Runtime bootstrapper** — auto-download Node.js and uv for npx/uvx agents. SHA256 verified, old versions cleaned up.
- **Inline skill references** — type `/` mid-prompt to reference skills.
- **Session meta hydration** — modes, models, commands, config, and plan state restored from persisted messages.
- **Tool call grouping** — live tool calls render as compact grouped blocks with streaming text pinned at bottom.
- **Window clamping** — app window clamped to screen size on launch.

### Changed

- **Settings UI tightened** — reduced padding, constrained width, collapsed CLI details.
- **Default window size** — `1720x1080` → `1440x900` for 14" MacBook Pro.
- **Release pipeline** — `finalize-release` job merges per-platform updater manifests.

### Fixed

- **Orphaned MCP processes** — `kill_on_drop(true)` on gateway child processes.
- **Session restore tool calls** — falls back to persisted `toolCallStart` messages.
- **SIZING.md compliance** — px-based layout violations converted to rem.

### Performance

- **`release_max_level_info`** — debug/trace compiled to no-ops in release builds.
- **Non-blocking stderr** — tracing stderr layer wrapped in `non_blocking()`.

### Security

- **SHA256 verification** — binary distributions verified against registry checksums.
- **App Store entitlements** — `files.bookmarks.app-scope` and `keychain-access-groups`.

---

## [0.1.5] — 2026-03-29

### Added

- Windows port with Azure Trusted Signing.
- Cross-platform CI pipeline.
- Refreshed app icons (iOS, Android, Windows asset sets).

---

## [0.1.3] — 2026-03-15

### Added

- Initial public release.
- ACP client management with carousel UI.
- MCP gateway with per-session HTTP.
- EventKit and AgentMail built-in MCP servers.
- Glass UI with dynamic accent colors.
- Session persistence and restore.

---

[Unreleased]: https://github.com/Weekendsuperhero-io/agent/compare/v0.3.14...HEAD
[0.3.8]: https://github.com/Weekendsuperhero-io/agent/compare/v0.3.7...v0.3.8
[0.3.7]: https://github.com/Weekendsuperhero-io/agent/compare/v0.3.6...v0.3.7
[0.3.6]: https://github.com/Weekendsuperhero-io/agent/compare/v0.3.4...v0.3.6
[0.3.4]: https://github.com/Weekendsuperhero-io/agent/compare/v0.3.3...v0.3.4
[0.3.3]: https://github.com/Weekendsuperhero-io/agent/compare/v0.3.1...v0.3.3
[0.3.1]: https://github.com/Weekendsuperhero-io/agent/compare/v0.3.0...v0.3.1
[0.3.0]: https://github.com/Weekendsuperhero-io/agent/compare/v0.2.2...v0.3.0
[0.2.2]: https://github.com/Weekendsuperhero-io/agent/compare/v0.2.1...v0.2.2
[0.2.1]: https://github.com/Weekendsuperhero-io/agent/compare/v0.1.5...v0.2.1
[0.1.5]: https://github.com/Weekendsuperhero-io/agent/compare/v0.1.3...v0.1.5
[0.1.3]: https://github.com/Weekendsuperhero-io/agent/releases/tag/v0.1.3
