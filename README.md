# Changelog

All notable changes to Agent are documented here. Each release section can be used directly as release notes.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Versions follow [Semantic Versioning](https://semver.org/).

---










## [Unreleased]

## [0.3.7] — 2026-04-02

## [0.3.6] — 2026-04-02

## [0.3.4] — 2026-04-01

## [0.3.3] — 2026-04-01

## [0.3.1] — 2026-04-01






## [Unreleased]

## [0.3.7] — 2026-04-02

## [0.3.6] — 2026-04-02

## [0.3.4] — 2026-04-01

## [0.3.3] — 2026-04-01

## [0.3.1] — 2026-04-01

## [0.3.0] — 2026-03-31

## [0.2.2] — 2026-03-31

## [0.2.1] — 2026-03-31










## [Unreleased]

## [0.3.7] — 2026-04-02

## [0.3.6] — 2026-04-02

## [0.3.4] — 2026-04-01

## [0.3.3] — 2026-04-01

## [0.3.1] — 2026-04-01






## [Unreleased]

## [0.3.7] — 2026-04-02

## [0.3.6] — 2026-04-02

## [0.3.4] — 2026-04-01

## [0.3.3] — 2026-04-01

## [0.3.1] — 2026-04-01

## [0.3.0] — 2026-03-31

## [0.2.2] — 2026-03-31

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

- **Settings UI tightened** — card padding reduced from `p-6` to `p-4` across all settings tabs. Content constrained to `max-w-5xl` so cards don't overflow the tab bar. Removed JS-based tab width measurement in favor of pure Tailwind.
- **Agent cards hide CLI details** — binary path and arguments collapsed by default in the carousel info card. Expandable for advanced users.
- **Agents tab matches ACP tab** — same card grid layout (`md:grid-cols-2 lg:grid-cols-3`), same card structure, same header with action buttons.
- **Settings buttons renamed** — "Add Client" / "Add Server" → "Custom" alongside "Browse Registry".
- **Default window size** — `1720x1080` → `1440x900` to fit 14" MacBook Pro without clamping.
- **Release pipeline** — macOS and Windows no longer race to publish. New `finalize-release` job merges per-platform `latest.json` updater manifests and publishes after all builds complete.
- **MCP server deletion** — calls `api.mcpRemove()` directly instead of relying on stale-closure `saveMCPSettings()`. Servers no longer respawn after deletion.

### Fixed

- **SIZING.md compliance** — audited and fixed all px-based layout violations: `text-[10px]` → `text-[0.625rem]`, `min-w-[16px]` → `min-w-4`, CSS `max-height`/`margin-left` converted to rem in widgets.css.
- **Orphaned MCP processes** — added `kill_on_drop(true)` to gateway child process spawning. Gateway `shutdown()` now calls `shutdown_arc_background()` instead of silently dropping the old manager.
- **Session restore tool calls** — `getBubble()` returning null for DB-loaded sessions no longer hides tool calls. Falls back to building state from the persisted `toolCallStart` message.
- **Streaming message render** — fixed TypeScript closure narrowing issue with `deferred.streamingMessage` pattern for pinned assistant text.
- **Clippy lint** — `let...else` patterns in `app-api/src/skills.rs`.

### Performance

- **`release_max_level_info`** — added to all three crates (`agent`, `eventkit-rs`, `agentmail-mcp`). `debug!()` and `trace!()` compile to no-ops in release builds.
- **Non-blocking stderr** — tracing stderr layer wrapped in `tracing_appender::non_blocking()`. Log I/O no longer blocks the main thread.
- **Chunk size warning** — bumped Vite `chunkSizeWarningLimit` to 7000. Icons chunk (6MB gzipped to 1.2MB) loads from local disk in Tauri — not a network concern.

### Security

- **SHA256 verification** — binary distributions verified against registry checksums on download. Computed hash stored in metadata for optional pre-run verification.
- **App Store entitlements** — added `files.bookmarks.app-scope` for persistent project directory access and `keychain-access-groups` for secret storage.

---

## [0.1.5] — 2026-03-29

### Added

- UI polish, tool call grouping, and session restore fixes
- Windows port with Azure Trusted Signing
- Cross-platform CI pipeline

### Changed

- Updated dependencies
- Refreshed app icons (iOS, Android, Windows asset sets)

---

## [0.1.3] — 2026-03-15

### Added

- Initial public release
- ACP client management with carousel UI
- MCP gateway with per-session HTTP
- EventKit and AgentMail built-in MCP servers
- Glass UI with dynamic accent colors
- Session persistence and restore

[Unreleased]: https://github.com/Weekendsuperhero-io/agent/compare/v0.3.7...HEAD
[0.1.5]: https://github.com/Weekendsuperhero-io/agent/compare/v0.1.3...v0.1.5
[0.1.3]: https://github.com/Weekendsuperhero-io/agent/releases/tag/v0.1.3
