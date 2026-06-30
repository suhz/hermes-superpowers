# Upstream Sync Status

This repository is a Hermes-native port of [obra/superpowers](https://github.com/obra/superpowers).

## Current sync base
- **Upstream version:** v6.0.3
- **Upstream commit:** `45c3cc5b66cfc5f147a7ddcfb86f7650e47a8ae0` (2026-06-18)
- **Hermes fork base:** upstream `917e5f5` (2026-04-06, between v5.0.7 and v5.1.0)
- **Last synced:** 2026-06-30

## Hermes transformation layer
Skills are adapted for the Hermes harness. Every ported skill has these transformations applied relative to upstream:
- **Layout/rename:** skill dirs/files `skills/<x>/` → `superpowers/superpowers-<x>/`; frontmatter `name: <x>` → `name: superpowers-<x>`.
- **Hermes-native tools:** `write_file`, `read_file`, `patch`, `search_files`, `find_files`, and `skill_view(name="superpowers-<x>")` replace other-harness tool names (e.g. Claude `Skill`/`TodoWrite`/`EnterPlanMode`).
- **Cross-refs:** `superpowers:<x>` → `superpowers-<x>`; `/superpowers:<x>` → `/superpowers-<x>`.
- **Paths:** `skills/<x>/<f>` → `superpowers/superpowers-<x>/<f>`; sibling `../<x>/<f>` → `../superpowers-<x>/<f>`.
- **Hermes-only dispatcher:** `superpowers/superpowers-slash-commands/` routes `/superpowers-<x>` commands via `skill_view()`.

## Deliberately not ported
- **Brainstorm visual-companion Node server** (`scripts/server.cjs`, `helper.js`, `frame-template.html`, `start-server.sh`, `stop-server.sh`): dropped. `visual-companion.md` is text-only (terminal-rendered visuals — mermaid, ASCII, tables).
- **Per-harness tool-mapping `references/` docs** under `using-superpowers` (`claude-code-tools.md`, `codex-tools.md`, `copilot-tools.md`, `gemini-tools.md`, `pi-tools.md`, `antigravity-tools.md`): not ported. `using-superpowers` tool-access sections are Hermes-native.
- **Other-harness plugin manifests, hooks, tests, release notes** (`.claude-plugin/`, `.codex-plugin/`, `hooks/`, `tests/`, `RELEASE-NOTES.md`, etc.): out of scope — Hermes ships only skills.

## Ported as skill-internal helpers (exception)
- `subagent-driven-development/scripts/{task-brief,review-package,sdd-workspace}`: copied verbatim. They are ≤1.4 KB harness-agnostic bash helpers required by the v6 SDD skill flow.

## How to re-sync
1. `cd ~/other/superpowers && git fetch --tags`
2. Diff each skill across the gap: `git diff <prev-base> <new-tag> -- skills/<skill>/`
3. Re-apply the Hermes transformation layer (above) to the new upstream content.
4. Update this file's "Current sync base" and "Last synced".
