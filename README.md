# Agent

A lightweight desktop app for working with [Agent Client Protocol (ACP)](https://agentclientprotocol.org) agents. Manage AI sessions, connect MCP servers, and interact with any ACP-compatible agent from a single workspace.

## Download

Head to [Releases](https://github.com/WeekendSuperhero/agent-releases/releases) and grab the latest version:

| Format | Description |
|--------|-------------|
| `Agent-v*.dmg` | macOS installer (universal — Apple Silicon + Intel) |
| `Agent-v*-universal-macos.zip` | macOS app bundle (zip) |

## Install

### macOS

1. Open the `.dmg` and drag **Agent** to your Applications folder
2. Launch Agent from Applications
3. If macOS shows a security prompt, go to **System Settings > Privacy & Security** and click **Open Anyway** — the app is code-signed and notarized by Apple

### Auto-Update

Agent checks for updates automatically. When a new version is available, you'll be prompted to download and install it in-place.

## Quick Start

### 1. Add an Agent

Click **+** to create an agent profile. You'll need:

- **Endpoint** — the URL of an ACP-compatible agent (e.g., `https://api.anthropic.com/v1/acp`)
- **API Key** — your authentication credentials

You can also configure a custom system prompt, default tasks, and environment variables.

### 2. Start a Session

Select your agent profile and start a new session. Agent supports multiple tabs so you can run several conversations side-by-side. All sessions are persisted locally — close and reopen the app without losing your work.

### 3. Connect MCP Servers (Optional)

Extend your agent's capabilities by connecting [Model Context Protocol](https://modelcontextprotocol.io) servers:

- **Settings > MCP Servers > Add Server**
- Supports stdio and SSE transports
- Import existing servers from Claude Desktop config with one click

### 4. Built-in Tools

Agent ships with two MCP servers that run in-process (no setup required):

- **EventKit** (macOS) — read and manage Calendar events and Reminders
- **AgentMail** — IMAP email client with multi-account support, search, and attachments

Enable them in **Settings > Built-in Servers**.

## Features

- **Multi-session workspace** — tab-based interface with full message history
- **Agent profiles** — reusable configurations with custom prompts and environment variables
- **MCP Gateway** — security boundary with policy-based tool access control and audit logging
- **Session intelligence** — probe agents for capabilities, switch models mid-conversation, auto-approval toggles
- **Cross-platform** — macOS, Linux, and Windows (macOS builds include translucent window effects)

## Requirements

- macOS 11 (Big Sur) or later
- Linux and Windows builds coming soon

## Links

- [Agent Client Protocol](https://agentclientprotocol.org)
- [Model Context Protocol](https://modelcontextprotocol.io)

## License

Proprietary — Copyright 2025-2026 WeekendSuperhero LLC. All rights reserved.
