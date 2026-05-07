# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

This repository **is** the source of truth for Claude Code's superpowers skills. The plugin clones it to `~/.config/superpowers/skills/`, where future agents read SKILL.md files at runtime. Edits here ship as instructions executed by other agents — wording is behavior.

The repo is mostly markdown, but several skills carry real code (TypeScript, shell scripts, graphviz). There is **no top-level build/test/lint** — validation is per-skill (commands below) plus human review on PRs.

## Repository layout

```
skills/
├── REQUESTS.md                 # community wishlist for new skills — check before authoring
├── using-skills/               # mandatory entry-point meta-skill (read first at session start)
├── architecture/
├── collaboration/
├── debugging/
├── meta/                       # skills about skills (writing-skills, testing-skills-with-subagents, gardening-skills-wiki)
├── problem-solving/
├── research/
└── testing/
```

Every skill lives at `skills/<category>/<kebab-case-name>/SKILL.md`. Filename is always `SKILL.md` (uppercase, exactly). A skill may have sibling files: `ABOUT.md` (attribution for derived work), `example.ts`, `*.sh`, `*.dot`, or a full `tool/` subdirectory with its own `package.json`. The README mentions a top-level `scripts/` dir that does not currently exist.

## Build / test / lint — per-skill, not repo-wide

There is no top-level `package.json`, `Makefile`, or CI. The commands that DO exist live inside individual skills:

| Tool | Path | Commands |
|------|------|----------|
| `conversation-search` (TypeScript + vitest) | `skills/collaboration/remembering-conversations/tool/` | `npm install` · `npm test` (vitest run) · `npm run test:watch` · `npm run index` · `npm run search` |
| Wiki gardening (shell) | `skills/meta/gardening-skills-wiki/` | `./garden.sh` · `./check-links.sh` · `./check-naming.sh` · `./check-index-coverage.sh` · `./analyze-search-gaps.sh` |
| Skill discovery (shell) | `skills/using-skills/` | `./find-skills [PATTERN]` · `./skill-run` |
| Root-cause helper (shell) | `skills/debugging/root-cause-tracing/` | `./find-polluter.sh` |

Run a single TypeScript test inside `tool/`: `npm test -- <substring>` (vitest filters by file/test name).

## Two kinds of edits in this repo, plus a boundary rule

### a) Editing prose (SKILL.md)
Bump frontmatter `version` (semver) on any non-trivial edit. Follow the authoring discipline defined in `skills/meta/writing-skills/SKILL.md` and the testing methodology in `skills/meta/testing-skills-with-subagents/SKILL.md`. Read those files when authoring or editing — do not rely on memory.

### b) Editing code inside a skill (TS, shell, graphviz, package.json)
Treat as normal code. Use ordinary code-TDD, follow that skill's existing patterns, and run that skill's test suite (commands above). If the code change alters behavior documented in SKILL.md, bump the SKILL.md `version` too.

### c) Boundary rule: prefer editing downstream code over editing a skill
Many problems an agent encounters in a downstream project are solved by changing code in **that project**, not by adding or editing a skill here. Don't reflexively reach for "let me add a skill." Run this checklist before touching this repo:

- Is the technique reusable across many projects? If **no** → fix the downstream code; don't edit skills here.
- Is the technique non-obvious and worth documenting for future agents? If **no** → fix the downstream code.
- Would another agent benefit from this as a reference next month? If **no** → fix the downstream code.
- Is this a one-off fix, project-specific convention, or standard practice documented elsewhere? If **yes** → fix the downstream code; do not add a skill.

## Adding a new skill

1. Search `skills/REQUESTS.md` to see if it's already requested or being discussed.
2. Pick a category. If none fits, propose a new category in the PR description rather than silently creating one.
3. Read `skills/meta/writing-skills/SKILL.md` and `skills/meta/testing-skills-with-subagents/SKILL.md` and follow them.
4. Match the shape of an existing peer skill in that category before inventing a new one.
5. If derived from external work, add a sibling `ABOUT.md`.

## Editing an existing skill

- Bump `version` in frontmatter on any non-trivial change.
- Re-test against the methodology in `skills/meta/testing-skills-with-subagents/SKILL.md`.
- Don't introduce `@` cross-references (they force-load files and burn 200k+ context). Use plain paths like `skills/category/skill-name`.
- If you're editing **code** inside the skill, run that skill's test suite from the commands table above.

## Session-start orientation

When starting a session in this repo, read `skills/using-skills/SKILL.md` first. It defines the mandatory workflow for the entire skills system. Do not duplicate or paraphrase its rules elsewhere — read the file.

## Git workflow

- Feature branches per session; the current session's branch is `claude/init-project-setup-odj3Z`. Develop, commit, and push there.
- PR-based contribution per `README.md`. Open PRs as **draft** by default.
- Commit messages: short imperative, optionally prefixed with `Fix:` / `Feat:` (see `git log --oneline` for examples).

## Common Mistakes

| Mistake | Why it matters |
|---------|---------------|
| Assuming "no build/test/lint" because the repo root has none | Several skills carry real code with their own test suites. Run those before claiming a change is verified. |
| Reflexively writing/editing a skill when a downstream code edit would do | Project-specific conventions belong in that project's CLAUDE.md, not here. |
| Copying skill content into this CLAUDE.md | Skills live in their own files; this CLAUDE.md is repo orientation only. Reference skill paths, don't duplicate them. |
| Using `@skills/.../SKILL.md` to cross-reference | `@` force-loads the file immediately, costing 200k+ context. Use plain paths. |
| Editing a skill without bumping `version` | Downstream installs can't tell something changed. |
| Creating a new category folder casually | Categories are the top-level taxonomy. Discuss in the PR before adding one. |
| Editing a skill without re-running its baseline test | Skills enforce behavior under pressure; untested edits break that. |
