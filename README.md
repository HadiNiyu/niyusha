# Niyusha Terminal

An AI coding agent that lives in your terminal. Niyusha reads and writes your workspace safely: every file change is **staged first**, you review a colored **diff**, and only then is it applied. It runs real shell commands behind a **permission** layer (allow/ask/deny), does web **search**, supports an **autonomous mode**, and more — with no heavy runtime, just plain Node.js (zero runtime dependencies).

```
Niyusha v0.1.4
Enter send · ↑↓/PgUp scroll · ←→ cursor · Ctrl+B side · { } / Alt+↑↓ side-scroll · Ctrl+S set · Esc stop
```

## Features

- **Staging workbench** — writes, edits, and deletes land in a virtual overlay; review the colored diff (`+`/`-`), then `/apply`, `/apply auto`, or `/reject`. Undo via `/undo` (even across restarts).
- **Mid-task approval** — as soon as something is staged, the AI pauses and waits for your decision; on approval it continues automatically, on rejection it builds a new strategy.
- **Real shell with permissions** — `run_command` runs on your host OS; pattern-based `allow`/`ask`/`deny` rules with dangerous-command detection (always confirmed) and once / session / **always** (persisted) options.
- **Access outside the workspace** — with permission (once/session), paths outside the workspace are handled for real; inside the workspace everything stays staged.
- **Autonomous mode** — `/auto on`: files are applied automatically (still `/undo`-able), non-dangerous shell is auto-approved; dangerous commands and outside-workspace paths still ask.
- **Web search** — `web_search` tool; settings via `/websearch` (on/off, provider, API key, limit). Public friendly defaults are built in (DuckDuckGo/Wikipedia); Tavily/Firecrawl/custom ready.
- **MCP client** — connect external MCP servers (stdio/HTTP) via `/mcp add|addurl|rm|on|off|trust`. Lean design: two thin tools (`mcp_list`, `mcp_call`), schemas are not injected into the prompt so tokens stay cheap. Servers come only from your config, calls go through the permission layer (untrusted asks; `trusted` runs freely), and MCP calls are blocked in plan mode.
- **Sessions** — save/load conversations plus working state (todos, staging, notes, background jobs): `/save`, `/load <id>`, the interactive picker `/sessions`, plus cross-workspace JSON export/import (`/sessions export|import`).
- **Memory** — `read_note`/`write_note` tools (per-workspace scratch notepad, secret-safe), skills, rules (project + user files), automatic context compaction, `/compact`.
- **TUI polish** — chat scroll separated from the input, numbered code boxes with collapse/expand, user-message highlighting, live markdown while streaming, mouse select (SGR), copy via `/copy`, **multi-language syntax highlighting** (JS/Python/HTML/CSS/Shell/JSON) inside code boxes, **truecolor brand gradient** (safe fallback on older terminals), **token-activity sparkline** + last-answer latency in the sidebar, a **mascot** whose face changes with state, **tool-activity timeline** (`● read_file · 262ms`), **chat search** (`/find`), **command palette** (`Ctrl+K`), automatic folding of long tool activity, **color themes** (`/theme`: dark-blue/matrix/cyberpunk/slate/midnight), **quick session switching** (`Alt+1..9`), **split diff viewer** (`/split`), **startup banner** with tips, **completion sound** (`/snd`: bell/double/ding, default off), **desktop notifications** (OSC 9/777) when a long task finishes, a **mini scrollbar** at the chat edge, **resizable sidebar** (`[`/`]`), **permission by word** (answer `oke`/`ya`/`tolak`, not only y/a/w/n), and **expandable reasoning** (`/reasoning`, `/expand`).

## Requirements

- **Node.js ≥ 18** (uses built-in `fetch`, `AbortSignal`, etc. — no runtime dependencies).

## Install

```bash
# From npm
npm i -g niyusha
niyusha

# From source
git clone https://github.com/HadiNiyu/niyusha.git
cd niyusha
npm i -g .        # the `niyusha` binary enters your PATH
# or run without installing:
node bin/niyusha.js

# Windows: niyusha.cmd   ·   macOS/Linux/WSL: ./niyusha.sh
```

Then start it in the folder you want to use as your workspace:

```bash
cd /path/to/your/project
niyusha
```

## Quick start

The first time: pick an AI provider and enter your API key (or use `/provider`, `/key`). Then just talk:

```
create a file src/hello.py that prints "hello"
start the server -> npm start
search for the latest react version
```

**Slash commands**: see `/help` for the full list. The common ones:

| Command | Purpose |
| --- | --- |
| `/provider` `/key` `/model` | Configure the AI provider |
| `/diff` `/apply [n]` `/apply auto` `/reject` | Review and decide on the staging |
| `/mouse [on\|off]` | Text selection with the mouse (auto-copy) |
| `/snd [on\|off\|bell\|double\|ding]` | Sound when the AI finishes: `bell` · `double` beep · `ding` (system sound). Default: off |
| `/sh <cmd>` | Run a shell command (same permissions as `run_command`) |
| `/undo` | Roll back the last change — files from `/apply` **or** shell effects (workspace restored) |
| `/perm` | Manage shell permissions |
| `/shell [auto\|cmd\|powershell\|pwsh\|bash\|wsl]` | Choose the shell for `run_command` (default auto + auto-WSL; fallback when missing) |
| `/auto [on\|off]` | Autonomous mode |
| `/plan <task>` | The AI makes a plan first (no file changes); then `[y]` execute · `[n]` revise · `[Esc]` dismiss — also offered automatically when the AI proposes a plan (`# RENCANA`) |
| `/websearch` | Web search settings |
| `/mcp` | External MCP servers: `ls` · `add <name> <cmd> [--trusted] [--env K=V]` · `addurl` · `rm` · `on\|off` · `trust` · `refresh` |
| `/copy [all]` | Copy the last answer / the whole chat to the clipboard |
| `/reasoning [n\|all\|collapse]` | Show/hide full reasoning (default collapsed; alias `/think`) |
| `/dev` | About & developer attribution (aliases `/about` · `/credits`) |
| `/expand [n]` `/collapse [n]` | Expand/collapse code and reasoning boxes |
| `/save` `/load <id>` | Save/load a session |
| `/sessions` | Interactive picker (Enter load · n new · s save · d delete) |
| `/sessions export\|import` | Export/import sessions to/from a JSON file (cross-workspace) |
| `/notes [clear]` `/ps` `/kill <pid>` | Notepad and background processes |
| `/compact` `/reset` | Context management |

**Keys**: `↑↓`/`PgUp/PgDn` scroll chat · `←→`/`Home`/`End`/`Ctrl+A/E` input cursor · `Ctrl+↑↓` history · `Ctrl+N` new session · `Ctrl+B` sidebar · `[` / `]` sidebar width · `{` / `}` or `Alt+↑↓` sidebar scroll (empty input) · `Ctrl+S` settings (clickable) · `Ctrl+G`/`/mouse` mouse selection · `Esc`/`Ctrl+C` stop.

## Configuration & storage

All state is local, in `~/.niyusha/`:

- `config.json` — provider, API key, permissions, web search.
- `state.json` — global history, skills, rules.
- `ws_<key>.json` — history, todos, sessions per workspace.
- `undo/` — snapshots used by `/undo` across restarts.
- `scratch/` — per-workspace notepad (read/written by the AI; `write_note` rejects secrets).
- `lastcopy.txt` — clipboard fallback.

Do not commit `~/.niyusha` (it contains your API key).

## Security model

1. **Stage first** — every file write/edit is staged; real data only changes after `/apply`.
2. **Layered permissions** — shell access and paths outside the workspace require approval (once/session/always); dangerous commands always confirm; `deny` rules keep blocking.
3. **Reversible** — applied changes are archived as snapshots, and non-read-only `run_command`/`/sh` effects are snapshotted automatically → `/undo` (works across restarts).
4. **Autonomous mode** only bypasses what can be recovered; anything that cannot be undone (destructive, outside paths) still goes through you.

## Development

```bash
node bin/niyusha.js      # run the TUI from source
```

This repository focuses on the **TUI** product (the `bin/` folder), which is also published to npm as `niyusha`. Zero runtime dependencies.

## License

MIT
