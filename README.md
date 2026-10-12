# Niyusha

**A featherweight AI coding agent that lives in your terminal.**
Staged edits with diff review · a permissioned real shell · web search · autonomous mode · sessions · subagents · MCP — in **~400 KB with zero runtime dependencies**.

[![npm version](https://img.shields.io/npm/v/niyusha.svg)](https://www.npmjs.com/package/niyusha)
[![license](https://img.shields.io/npm/l/niyusha.svg)](https://www.npmjs.com/package/niyusha)

## Install & update

```bash
npm i -g niyusha          # install
niyusha                   # run it inside any project

npm i -g niyusha@latest   # update (or: npm update -g niyusha)
niyusha -v                # confirm the version
```

No build step, no daemon, no config files to write — it starts in milliseconds.

---

## Why Niyusha

Most tooling today wants a framework, a compiler, and a hundred packages. Niyusha doesn't.

- **Ridiculously light** — pure Node.js, **zero runtime dependencies**, tiny install footprint.
- **Instant** — no bundling, no service to boot; run it and you're working.
- **Lean by design** — a compact, hand-tuned system prompt + tools, protected by a **token-budget guard** so the per-request overhead can't quietly creep up.
- **Safe by default** — every file change is **staged first**; you approve a colored diff before anything touches disk.
- **Truly reversible** — `/undo` reverts applied files *and* real shell side effects, across restarts.
- **Delegates** — read-only **subagents** for broad exploration, or `build` subagents whose edits land in the same review queue.
- **Real, not sandboxed theatre** — it actually reads, edits, and runs commands in your workspace.
- **Local & private** — your code, sessions, and history stay on your machine (`~/.niyusha/`).

## Requirements

- **Node.js 18+** — that's the only prerequisite.

## Bring your own model 🔑

Niyusha is **model-agnostic**. You supply an **LLM API key** — **free or paid** — and pick a provider once:

```bash
/provider     # choose a provider
/key          # paste your API key
/model        # it auto-loads the available models from that provider
```

Prefer free? Several providers ship generous **free tiers**, and a **local Ollama** needs **no key at all**. Bring whatever you already have — cloud or local — and Niyusha adapts.

## Quick start

```bash
cd /path/to/your/project
niyusha
```

Then just talk:

```
explain how auth works in @/src/api/index.ts
add a dark mode toggle to the settings page and run the tests
run /init          # generates an AGENTS.md for this repo
search for the latest React release
```

Type `@` + `Tab` to reference files, attach images with `/img`, and run `/help` anytime.

## Built-in tools (and yes, there are a lot)

Niyusha comes with **20+ tools** out of the box — no plugins to install:

- **Files** — `read_file` · `write_file` · `edit_file` · `rename_file` · `delete_file` · `list_directory` · `create_directory`
- **Search & explore** — `glob_files` · `grep_search` · `workspace_tree`
- **Execute** — `run_command` (real shell, permissioned) · `run_code`
- **Research** — `web_search` (works with no extra key)
- **Plan & remember** — `set_todos` · `read_note` / `write_note` / `clear_note`
- **Extend** — `use_skill` · `task` (delegate to a **subagent**: `explore` read-only / `build` that stages edits for review) · `mcp_list` / `mcp_call` (connect any **MCP server**)

…plus **skills**, **project rules** (`AGENTS.md`), **session save/load**, `@file` references, image attach, `/init`, `/redo`, split diff, themes and a full command palette. The list keeps growing.

## Safety you can see

1. **Stage first** — writes/edits/deletes land in a virtual overlay; nothing is real until you `/apply`.
2. **Layered permissions** — shell commands and outside-workspace paths ask first (once / session / always); dangerous commands always confirm.
3. **Reversible** — `/undo` rolls back applied files *and* shell side effects, across restarts. `/redo` re-applies.
4. **Autonomous mode** (`/auto on`) only skips what can be recovered — destructive actions still stop for you.

## Sessions, memory & freedom

- Save and resume full conversations **with working state** — todos, staging, notes, background jobs.
- Export/import sessions as portable JSON, even **across different projects**.
- Talks like you do: it replies in **your language**, whatever that is.

## Links

- **npm:** https://www.npmjs.com/package/niyusha
- **License:** MIT

---

*Small on disk. Serious about getting work done.*
