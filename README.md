# Agent Settings

This repository stores portable principles for configuring Codex and agent-friendly projects. It intentionally does not store machine-specific Codex state.

## Core Principle

Use `CLAUDE.md` as the default project memory file for Codex.

Codex should treat `CLAUDE.md` and `AGENTS.md` as equivalent project instruction files, with `CLAUDE.md` preferred for new projects and durable project memory.

The goal is to avoid isolated topic threads inside the same project. `CLAUDE.md` should act as the shared project memory surface across future Codex sessions, Claude Code sessions, and other coding-agent workflows.

## Codex Setup On A New Machine

Ask Codex on that machine to apply this repository's rules locally.

The desired local Codex behavior is:

- Use `CLAUDE.md` as the default project memory file.
- Treat `CLAUDE.md` and `AGENTS.md` as equivalent project instruction files.
- Read project instruction files before substantial exploration or edits.
- Create a root `CLAUDE.md` by default when creating a new project.
- Do not create `AGENTS.md` unless explicitly requested or when OpenAI/Codex-native compatibility is required.
- Update `CLAUDE.md` when durable project context changes.
- Keep `CLAUDE.md` concise, practical, and agent-facing.
- Record only stable, reusable information: purpose, verified commands, important files, conventions, user preferences, known pitfalls, and durable decisions.

The local Codex config should include this setting:

```toml
project_doc_fallback_filenames = ["CLAUDE.md", "README.md"]
```

Codex global instructions should include:

```md
# Personal Agent Instructions

Use `CLAUDE.md` as the default project memory file for this user.

When working in a repository:
- Treat `CLAUDE.md` and `AGENTS.md` as equivalent project instruction files.
- Look for `CLAUDE.md` and `AGENTS.md` from the current working directory upward to the repository root.
- Read and follow them before substantial exploration or edits.
- If both exist, follow both unless they conflict.
- If they conflict, prefer the more specific file for the current path; if still ambiguous, ask the user.

When creating a new project:
- Create a root `CLAUDE.md` by default.
- Do not create `AGENTS.md` unless the user explicitly asks for OpenAI/Codex-native project instructions or cross-agent compatibility requires it.
- Keep the initial `CLAUDE.md` short and Claude Code style.

When maintaining project memory:
- Update `CLAUDE.md` when durable project context changes.
- Durable context includes project purpose, verified run/test/build commands, important files, stable conventions, user preferences, known pitfalls, and decisions future agents should inherit.
- Do not update `CLAUDE.md` for every minor action, temporary thought, or obvious fact visible from the code.
- Prefer updating existing `CLAUDE.md` over creating another instruction file.
- If only `AGENTS.md` exists, follow it and consider creating or updating `CLAUDE.md` when starting long-lived project work.

Style rules for `CLAUDE.md`:
- Keep it concise, practical, and agent-facing.
- Target 20-80 lines; avoid exceeding 100 lines unless explicitly necessary.
- Prefer bullets over prose.
- Do not duplicate README content except for commands or conventions agents must know.
- Do not write generic filler such as "follow best practices" or "keep code clean."
- Record only stable, reusable information.
- For a new project's initial `CLAUDE.md`, include only sections that already have useful content.
- Prefer this rough shape when relevant: Purpose, Commands, Key Files, Working Rules, Memory.
- Omit empty sections and avoid placeholders such as `TBD` or `<path>`.
```

## Chinese/UTF-8

On Windows/PowerShell, always use explicit UTF-8 for Chinese text files: `Get-Content -Raw -Encoding UTF8` for reads, explicit UTF-8 for writes, and UTF-8 for Python/Node text I/O.

Use English for code identifiers, file names, config keys, log events, and machine-readable fields. Use Chinese only for docs, prompts, user-facing copy, and business data.

Prefer project scripts over ad hoc PowerShell commands for Chinese-heavy files.

## What Not To Sync

Do not commit a real `~/.codex` directory or a full `config.toml` copied from a machine.

Avoid syncing:

- `auth.json`
- `sessions/`
- `archived_sessions/`
- logs and SQLite databases
- cache and tmp directories
- sandbox directories
- plugin marketplace cache paths
- trusted project paths
- machine-specific absolute paths

## Suggested Prompt For A New Computer

```text
Read this repository's README.md and configure this computer's local Codex settings accordingly. Do not sync secrets, logs, caches, trusted project paths, or machine-specific absolute paths. Apply only the portable behavior rules: use CLAUDE.md as the default project memory file, treat CLAUDE.md and AGENTS.md as equivalent project instruction files, configure Codex fallback filenames, and write the matching global Codex instructions.
```

