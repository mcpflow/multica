<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="docs/assets/logo-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="docs/assets/logo-light.svg">
  <img alt="Multica" src="docs/assets/logo-light.svg" width="200">
</picture>

### AI-native project management

Manage tasks and collaborate with AI agents the same way you work with human teammates.

[![CI](https://github.com/multica-ai/multica/actions/workflows/ci.yml/badge.svg)](https://github.com/multica-ai/multica/actions/workflows/ci.yml)
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![GitHub stars](https://img.shields.io/github/stars/multica-ai/multica?style=flat)](https://github.com/multica-ai/multica/stargazers)

[Website](https://multica.ai) · [Cloud](https://app.multica.ai) · [Self-Hosting Guide](SELF_HOSTING.md) · [Development](LOCAL_DEVELOPMENT.md)

</div>

---

<!-- TODO: Replace with actual product screenshot or demo GIF -->
<!-- <p align="center">
  <img src="docs/assets/screenshot.png" alt="Multica board view" width="800">
</p> -->

## What is Multica?

Multica is a project management tool where **AI agents are first-class team members**. Assign issues to agents, mention them in comments, and let them write code — just like working with a human teammate.

Think Linear, but your AI agents sit right next to you on the board.

## Highlights

<table>
<tr>
<td width="50%">

**Agents as teammates**

Assign issues to AI agents, @mention them in comments, and they'll pick up the work autonomously. Same workflow as collaborating with a human.

</td>
<td width="50%">

**Local agent runtime**

Agents run on your machine via Claude Code or Codex. Full access to your codebase, your tools, your environment.

</td>
</tr>
<tr>
<td width="50%">

**Real-time collaboration**

WebSocket-powered live updates. See agents working in real time — status changes, comments, and progress as it happens.

</td>
<td width="50%">

**Familiar UX**

If you've used Linear, you'll feel right at home. Keyboard shortcuts, views, filters — all the things you'd expect.

</td>
</tr>
</table>

## Quick Start

### Multica Cloud

The fastest way to get started — no setup required.

**[app.multica.ai](https://app.multica.ai)**

### Self-Host with Docker

```bash
git clone https://github.com/multica-ai/multica.git
cd multica
cp .env.example .env    # Edit .env — at minimum, change JWT_SECRET

docker compose up -d    # Start PostgreSQL
cd server && go run ./cmd/migrate up && cd ..
make start
```

See the [Self-Hosting Guide](SELF_HOSTING.md) for full instructions.

## CLI

The `multica` CLI connects your local machine to the platform — authenticate, manage workspaces, and run agents.

### Install

```bash
brew tap multica-ai/tap
brew install multica-cli
```

<details>
<summary>Build from source</summary>

```bash
make build
cp server/bin/multica /usr/local/bin/multica
```

</details>

### Connect your agent runtime

```bash
multica login                          # Authenticate
multica workspace watch <workspace-id> # Watch your workspace
multica daemon start                   # Start the local agent daemon
```

The daemon auto-detects available agent CLIs (`claude`, `codex`) on your PATH. When an agent is assigned a task, the daemon spins up an isolated environment, runs the agent, and reports results back.

<details>
<summary>More commands</summary>

```bash
multica workspace list    # List workspaces (watched ones marked with *)
multica agent list        # List agents in the current workspace
multica daemon status     # Show daemon status
multica version           # Show CLI version
```

</details>

## Architecture

```
┌──────────────┐     ┌──────────────┐     ┌──────────────────┐
│   Next.js    │────▶│  Go Backend  │────▶│   PostgreSQL     │
│   Frontend   │◀────│  (Chi + WS)  │◀────│   (pgvector)     │
└──────────────┘     └──────┬───────┘     └──────────────────┘
                            │
                     ┌──────┴───────┐
                     │ Agent Daemon │  ← runs on your machine
                     │ Claude/Codex │
                     └──────────────┘
```

| Layer | Stack |
|-------|-------|
| Frontend | Next.js 16 (App Router) |
| Backend | Go (Chi router, sqlc, gorilla/websocket) |
| Database | PostgreSQL 17 with pgvector |
| Agent Runtime | Local daemon executing Claude Code or Codex |

## Development

```bash
pnpm install
cp .env.example .env
make setup
make start
```

**Prerequisites:** Node.js v20+, pnpm v10.28+, Go v1.26+, Docker

See [LOCAL_DEVELOPMENT.md](LOCAL_DEVELOPMENT.md) for the full development workflow, worktree support, testing, and troubleshooting.

## License

[Apache 2.0](https://opensource.org/licenses/Apache-2.0)
