# Niyusha

An AI coding agent that lives in your terminal — staged file edits with diff review, permissioned shell, web search, autonomous mode, sessions, and an MCP client. Zero runtime dependencies.

## Install

Requirements: **Node.js ≥ 18**.

```bash
npm i -g niyusha
```

## Quick start

```bash
cd /path/to/your/project
niyusha
```

On first run, pick an AI provider and enter your API key (`/provider`, `/key`). Then just talk:

```
create a file src/hello.py that prints "hello"
start the server -> npm start
search for the latest react version
```

## Highlights

- **Staged edits** — every write/edit is a virtual overlay; review the diff, then `/apply`, or `/undo`.
- **Permissioned shell** — real `run_command` behind allow/ask/deny rules; dangerous commands always ask.
- **Autonomous mode** — `/auto on`: files apply automatically (still undoable).
- **Web search** — `/websearch` (on/off, provider, API key).
- **MCP client** — `/mcp add|addurl|rm|on|off|trust`.
- **Sessions** — `/save`, `/load`, `/sessions`, JSON export/import.
- **Themes** — `/theme`: dark-blue/matrix/cyberpunk/slate/midnight.

## Common commands

| Command | Purpose |
| --- | --- |
| `/provider` `/key` `/model` | Configure the AI provider |
| `/diff` `/apply` `/reject` `/undo` | Review and decide on the staging |
| `/perm` | Manage shell permissions |
| `/auto [on\|off]` | Autonomous mode |
| `/plan <task>` | Read-only plan first |
| `/mcp` | Manage MCP servers |
| `/sessions` `/save` `/load` | Sessions |
| `/copy [all]` | Copy last answer / whole chat |
| `/theme` `/snd` `/find` | Polish & tools |

See `/help` for the full list.

## Security model

1. **Stage first** — data only changes after `/apply`.
2. **Layered permissions** — shell + outside-workspace paths need approval (once/session/always).
3. **Reversible** — `/undo` across restarts.
4. **Autonomous mode** only bypasses what can be recovered.

All local state (config, history, undo) lives in `~/.niyusha/` — never commit that folder (it holds your API key).

## Links

- npm: https://www.npmjs.com/package/niyusha
- License: MIT
