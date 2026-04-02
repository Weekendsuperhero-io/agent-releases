# Changelog

All notable changes to Agent are documented here. Each release section can be used directly as release notes.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Versions follow [Semantic Versioning](https://semver.org/).

---


## [Unreleased]

## [0.3.8] — 2026-04-02

### Added

- **Spawn failure diagnostics** — cross-platform diagnostics when a child process fails to launch: file existence, permissions, PATH resolution, shebang/interpreter checks. On macOS, also logs quarantine xattr, Gatekeeper assessment, code signature, and sandbox entitlement status.
- **Startup entitlements dump** — logs the app's effective entitlements at startup via Security.framework (`security_debug.rs`) for TestFlight/App Store debugging.
- **`ENTITLEMENTS.md`** — reference doc for both entitlement profiles, explaining every key and what was intentionally excluded.

### Changed

- **unknownUpdate event type** — `event_type_str()` now returns the actual event type from the raw JSON instead of hardcoded `"unknownUpdate"`, making unknown events queryable by their real type in `session_messages`.

### Fixed

- **Cmd+C / Ctrl+C copy** — keyboard copy wasn't reaching the WebView's native copy handler. Added a global keydown handler that reads `window.getSelection()` and writes plain text to the clipboard via the Tauri clipboard plugin. Works cross-platform.
- **Registry agent runtime PATH** — Node.js agents installed from the registry failed to launch because `_extra_paths` (bootstrapped runtime bin dirs from `metadata.extra_paths`) wasn't being injected into spawn env vars. Fixed in both the backend (Tauri command layer reads metadata from DB) and frontend (`buildAgentEnv` helper extracts it for probe/list calls).

### Security

- **App Store entitlements overhaul** — added `files.user-selected.executable` (non-quarantined runtime writes), `privileged-file-operations` (symlinks, file replacement), media folder access (pictures, music, movies), `device.audio-input` (speech-to-text), and `automation.apple-events` (AppleScript). Removed incorrect `com.apple.security.inherit` (only valid on child targets). Removed unnecessary hardened runtime exceptions (`allow-jit`, `allow-unsigned-executable-memory`, `disable-library-validation`) — WKWebView handles JIT in its own process.

---

## [0.3.7] — 2026-04-02

### Fixed

- **Process spawning** — fixed child process lifecycle, session cleanup, and App Sandbox support.

---

## [0.3.6] — 2026-04-02

### Fixed

- Minor bug fixes and stability improvements.

---

## [0.3.4] — 2026-04-01

### Changed

- Internal improvements and dependency updates.

---

## [0.3.3] — 2026-04-01

### Changed

- Internal improvements and dependency updates.

---

## [0.3.1] — 2026-04-01

### Changed

- Internal improvements and dependency updates.

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

- **Registry browser** — browse and install ACP agents and MCP servers from their official registries. ACP registry loads in full; MCP registry searches server-side (4+ character query). Distribution type badges (binary, npx, uvx, http, docker) shown on every entry.
- **Runtime bootstrapper** — Node.js and uv are automatically downloaded when installing npx/uvx agents or servers. Resolves latest LTS (node) and latest release (uv) dynamically. SHA256 verified against official checksums. Old versions cleaned up after update.
- **Inline skill references** — type `/` mid-prompt to reference a skill. Inserts `/"skill-name"` syntax, highlighted in the input, sent as a file reference through acp-fs.
- **Session meta hydration** — modes, models, commands, config, and plan state are now restored from persisted messages when loading sessions from the database.
- **Tool call grouping** — live tool calls during a turn render as a compact grouped block. Streaming assistant text stays pinned at the bottom.
- **Tool calls in restored sessions** — DB-loaded sessions now render tool calls correctly via static fallback when the live hook is empty.
- **Browse Registry button** — available in the ACP carousel, ACP settings tab, and MCP settings tab.
- **Cancel on new agent/server** — clicking back while adding a new ACP agent or MCP server now discards the unsaved entry instead of saving an empty one.
- **Window clamping** — app window is clamped to screen size on launch so it never opens off-screen (14" MacBook Pro fix).
- **`SOUL.md`** — project soul document defining principles, architecture rules, and vision.
- **`SANDBOXING.md`** — documents current cap-std file sandbox and future OS-level process sandboxing (Seatbelt, Bubblewrap).
- **`SESSION_MAPPING_AND_SECURITY.md`** — planned token-based single-port gateway architecture.
- **`registry_urls.toml`** — all registry and runtime source URLs in one file, read at compile time.

### Changed

- **Settings UI tightened** — card padding reduced from `p-6` to `p-4` across all settings tabs. Content constrained to `max-w-5xl` so cards don't overflow the tab bar.
- **Agent cards hide CLI details** — binary path and arguments collapsed by default in the carousel info card.
- **Agents tab matches ACP tab** — same card grid layout, same card structure, same header with action buttons.
- **Settings buttons renamed** — "Add Client" / "Add Server" → "Custom" alongside "Browse Registry".
- **Default window size** — `1720x1080` → `1440x900` to fit 14" MacBook Pro without clamping.
- **Release pipeline** — new `finalize-release` job merges per-platform `latest.json` updater manifests and publishes after all builds complete.
- **MCP server deletion** — calls `api.mcpRemove()` directly instead of relying on stale-closure `saveMCPSettings()`.

### Fixed

- **SIZING.md compliance** — audited and fixed all px-based layout violations.
- **Orphaned MCP processes** — added `kill_on_drop(true)` to gateway child process spawning.
- **Session restore tool calls** — `getBubble()` returning null for DB-loaded sessions no longer hides tool calls.
- **Streaming message render** — fixed TypeScript closure narrowing issue with `deferred.streamingMessage`.

### Performance

- **`release_max_level_info`** — `debug!()` and `trace!()` compile to no-ops in release builds.
- **Non-blocking stderr** — tracing stderr layer wrapped in `tracing_appender::non_blocking()`.

### Security

- **SHA256 verification** — binary distributions verified against registry checksums on download.
- **App Store entitlements** — added `files.bookmarks.app-scope` and `keychain-access-groups`.

---

## [0.1.5] — 2026-03-29

### Added

- UI polish, tool call grouping, and session restore fixes.
- Windows port with Azure Trusted Signing.
- Cross-platform CI pipeline.

### Changed

- Updated dependencies.
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

[Unreleased]: https://github.com/Weekendsuperhero-io/agent/compare/v0.3.8...HEAD
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
