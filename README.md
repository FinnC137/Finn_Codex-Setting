# Agent Settings

This repository stores portable principles for configuring Codex and agent-friendly projects. It intentionally does not store machine-specific Codex state.

## Core Principle

For long-lived projects, use `CLAUDE.md` as the primary project-level agent instruction file.

Codex should be configured to treat `CLAUDE.md` as a project instruction fallback, so projects remain compatible with Claude Code while still being usable by Codex and other coding agents.

## Codex Setup On A New Machine

Ask Codex on that machine to apply this repository's rules locally.

The desired local Codex behavior is:

- Read and follow `CLAUDE.md` if present in a project.
- Prefer updating `CLAUDE.md` instead of creating a separate `AGENTS.md`.
- Only create `AGENTS.md` when explicitly requested or when OpenAI/Codex-native compatibility is required.
- Keep project instructions lightweight: project purpose, key files, install/run/test/build commands, working rules, and verification steps.

The local Codex config should include this setting:

```toml
project_doc_fallback_filenames = ["CLAUDE.md", "README.md"]
```

Codex global instructions should include:

```md
# Personal Agent Instructions

For long-lived projects, use `CLAUDE.md` as the primary project-level agent instruction file.

When working in a repository:
- Read and follow `CLAUDE.md` if present.
- Prefer updating `CLAUDE.md` instead of creating a separate `AGENTS.md`.
- Only create `AGENTS.md` when the user explicitly asks for OpenAI/Codex-native project instructions or cross-agent compatibility requires it.
- Keep project instructions lightweight: project purpose, key files, install/run/test/build commands, working rules, and verification steps.
```

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
Read this repository's README.md and configure this computer's local Codex settings accordingly. Do not sync secrets, logs, caches, trusted project paths, or machine-specific absolute paths. Apply only the portable behavior rules: use CLAUDE.md as the primary project instruction file, configure Codex fallback filenames, and write the matching global Codex instructions.
```

