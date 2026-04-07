<p align="center">
  <a href="https://rootcx.com">
    <img src="https://rootcx.com/logo.svg" width="60" />
  </a>
</p>

<h3 align="center">RootCX Plugin for Claude Code</h3>

<p align="center">
Build and deploy RootCX apps and AI agents directly from Claude Code.
</p>

<p align="center">
  <a href="https://rootcx.com/docs"><img src="https://img.shields.io/badge/docs-rootcx.com-blue" alt="Documentation" /></a>
  <a href="https://discord.gg/W7sqMYtdws"><img src="https://img.shields.io/discord/1472936179383930950?color=5865F2&label=Discord&logo=discord&logoColor=white" alt="Discord" /></a>
  <a href="https://github.com/rootcx/rootcx/stargazers"><img src="https://img.shields.io/github/stars/rootcx/rootcx?style=social" alt="Stars" /></a>
</p>

<p align="center">
  <a href="https://rootcx.com">Website</a> · <a href="https://rootcx.com/docs">Docs</a> · <a href="https://discord.gg/rootcx">Community</a> · <a href="https://rootcx.com/docs/guides/getting-started">Get Started</a>
</p>

<br />

## What is this?

The official [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugin for [RootCX](https://rootcx.com), the open-source platform for building internal software and AI agents.

Connect to any RootCX Core (local or cloud), scaffold full-stack projects, and deploy. All through natural language, directly from your terminal. No IDE required.

## Quick Start

### 1. Install the plugin

```
/plugin marketplace add RootCX/claude-code-plugin
/plugin install rootcx
```

### 2. Install the CLI

Download the `rootcx` binary from [Releases](https://github.com/RootCX/RootCX/releases) and add it to your PATH.

### 3. Connect and build

```
/rootcx:connect http://localhost:9100
```

Authentication is automatic: password, OIDC SSO, or no-auth for local dev.

```
/rootcx:new app my-crm
```

Claude scaffolds a complete project with Vite, React 19, Tailwind v4, `@rootcx/sdk` and `@rootcx/ui`.

```
/rootcx:deploy
```

Manifest sync, backend upload, frontend deploy, worker start. One command.

## Commands

| Command | What it does |
|---------|-------------|
| `/rootcx:connect <url>` | Connect to a RootCX Core and authenticate |
| `/rootcx:new <app\|agent> <name>` | Scaffold a new app or AI agent project |
| `/rootcx:deploy` | Deploy the current project to the connected Core |

## Skills

Six specialized skills that teach Claude Code how to build for RootCX. Loaded automatically when relevant.

| Skill | Claude Code learns |
|-------|-------------------|
| **rootcx-manifest** | `manifest.json`: entities, field types, entity links, RBAC permissions, schema sync |
| **rootcx-sdk-hooks** | `@rootcx/sdk`: `useAppCollection`, `useAppRecord`, `useIntegration`, query operators |
| **rootcx-ui** | `@rootcx/ui`: AppShell, Sidebar, DataTable, forms, dark mode, AuthGate |
| **rootcx-rest-api** | Core REST API: collections CRUD, bulk ops, integrations, async jobs |
| **rootcx-backend-worker** | Backend workers: Bun IPC protocol, RPC handlers, job processing |
| **rootcx-agent** | AI agents: `agent.json`, LangGraph, IPC bridge, tool-calling with RBAC |

## What You Can Build

### Apps

Full-stack internal tools with a React frontend, managed PostgreSQL, and optional backend workers. CRMs, dashboards, admin panels, inventory systems.

```
You: Create a CRM with contacts, companies, and deals.
     Add a kanban view for deals grouped by pipeline stage.

Claude: Generates manifest.json with 3 entities and entity links,
        scaffolds React views with DataTable and Kanban,
        deploys to your connected Core.
```

### AI Agents

LangGraph-powered agents with secure tool access to your app data, streaming chat UI, and full RBAC enforcement.

```
You: Build a support agent that searches our ticket database,
     suggests solutions, and can escalate to humans.

Claude: Generates agent.json with system prompt and tool config,
        scaffolds LangGraph backend with query and escalation tools,
        creates chat UI with streaming responses and tool activity.
```

## How It Works

```
Claude Code
  /rootcx:* commands      call the rootcx CLI via Bash
  Skills (6)               loaded on demand for deep platform knowledge

  rootcx CLI (Rust)        handles auth, scaffold, archive, deploy
      |
      | HTTP
      v
  RootCX Core (:9100)     local or remote
```

No MCP server. No background process. The plugin calls a fast native Rust binary that talks to the Core REST API. Same protocol as Studio.

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (CLI, desktop app, or IDE extension)
- [`rootcx` CLI](https://github.com/RootCX/RootCX/releases) in your PATH
- A RootCX Core: [cloud](https://rootcx.com/app/register) or [self-hosted](https://rootcx.com/docs/developers/self-hosting)

## Community

- [Discord](https://discord.gg/rootcx) for questions, discussion, and support
- [GitHub Issues](https://github.com/rootcx/rootcx/issues) for bugs and feature requests
- [Documentation](https://rootcx.com/docs) for guides, references, and API docs

## License

Apache 2.0
