---
name: repo-onboarding
description: "Use at the start of work in any repository, especially one opened for the first time or not touched recently. Auto-bootstraps AI-assisted tooling (agent rules, Serena, knowledge graph) when missing, then hands off to the user's real task."
---

# Repo Onboarding (auto-bootstrap)

Run this checklist before deep work in a repository. Repo-agnostic: detect everything from what is present; never assume a stack or project name.

## 1. Coding-convention skill + agent rules

Order of precedence:

1. Repo-local skills: `.opencode/skills/`, `.agents/skills/` — read any coding-convention skill found there (e.g. a backend/framework conventions skill) and follow it.
2. Repo rules files: `AGENTS.md`, `CLAUDE.md` — read and follow.
3. If neither exists, fall back to the global `coding-conventions` skill: it detects the stack (backend or frontend, any language) and applies top-tier structure for it.
4. If the repo is substantial and has no rules file or conventions skill, generate a concise `AGENTS.md` from what you detected (commands, test/lint, structure) and show it to the user — do not silently commit it.

Never invent conventions that contradict what is already in the repo.

## 2. Serena (semantic code retrieval) — auto-install

Serena runs globally with project auto-detection from the working directory.

- If the session has Serena tools: confirm the active project matches the current repo; activate the current directory if not. `.serena/project.yml` is created automatically on first activation — no manual setup.
- If Serena is not installed at all: install it globally once (`uv tool install` / `uvx` from the serena repo) or tell the user the one-liner if installs are restricted; then re-check.
- If the repo's primary language has no Serena LSP backend (Python, TS/JS, Go, Java, C#, Rust, Ruby, PHP are supported), skip Serena silently and use normal file tools.
- `.serena/` is local cache: never commit it; add to `.git/info/exclude` if not already ignored.

## 3. Knowledge graph (Graphify) — auto-install/build

- If `graphify-out/graph.json` exists: prefer `graphify query "<question>"` / `path` / `explain` over broad grep or full-file reads; use `graphify-out/wiki/index.md` for navigation when present.
- If missing: build it automatically when the repo is large (many modules or > ~200 source files): `graphify update .` then `graphify export wiki` (AST-only, no API cost). Run it in the background if the user has an urgent task; otherwise do it now.
- If the `graphify` CLI is missing, install it once (`uv tool install graphifyy`) or give the user the one-liner.
- Respect `.graphifyignore`. Dirty `graphify-out/` files after hooks are normal — never treat them as errors.
- Never commit `graphify-out/` unless the repo already tracks it; check `.gitignore` first.

## 4. Local state sanity

- Match the repo's documented setup before running commands (venv vs container, lockfiles).
- Find test + lint commands from `Makefile` / `package.json` / `pyproject.toml`; run scoped checks only.
- Leave untracked dumps/binaries alone; ask before touching them.

## 5. Boy scout rule (incremental cleanup)

Existing mess is not yours to fix in bulk. Clean up **only files and functions the current task already touches**: dead imports, naming, structure, missing tests for changed paths. Never produce repo-wide reformat/refactor diffs. The codebase gets clean gradually, one touched file at a time.

## Output

Report onboarding as one short checklist: conventions source (repo skill / AGENTS.md / global skill / generated), serena (active / installed / n/a), graph (present / built / skipped), plus anything requiring the user. Then proceed with the actual task.
