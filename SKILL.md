---
name: Working in the superpowers-skills repo
description: Orientation for agents editing the superpowers skills library — what this repo is, where things live, and what NOT to do
when_to_use: when starting a session inside the superpowers-skills repository, or about to add, edit, or move any skill in it
version: 0.1.0
languages: all
---

# Working in the superpowers-skills repo

## Overview

This repository **is** the source of truth for Claude Code's superpowers skills. The plugin clones it to `~/.config/superpowers/skills/`, where future agents read SKILL.md files at runtime. **Edits here ship as instructions executed by other agents** — wording is behavior, so treat changes with the care you'd give to code.

The repo contains only markdown. There is no build step, no test suite, no linter, no CI. Validation is by human review on PRs.

## When to Use

Read this skill when:

- A session starts in `/home/user/superpowers-skills` (or wherever the repo lives)
- You're about to create a new skill, edit an existing one, or restructure categories
- You're orienting before answering a question about repo conventions
- You hit "is there a build/test command?" — answer is here

Do NOT use this skill as a substitute for `skills/meta/writing-skills` when actually authoring a skill — that one is the authoring discipline (TDD for skills). This file is just orientation.

## Quick Reference

| Topic | Answer |
|-------|--------|
| What is this repo? | The skills library that the superpowers plugin clones to `~/.config/superpowers/skills/` |
| Build / test / lint? | None. Markdown only. |
| Where do skills live? | `skills/<category>/<kebab-case-name>/SKILL.md` |
| Required filename | `SKILL.md` (uppercase, exactly) |
| Mandatory entry-point skill | `skills/using-skills` — read at session start |
| Authoring discipline | `skills/meta/writing-skills` — TDD for skills, has Iron Law |
| Categories in use | architecture, collaboration, debugging, meta, problem-solving, research, testing, using-skills |
| New skill ideas | Check `skills/REQUESTS.md` first |
| Attribution for derived skills | Sibling `ABOUT.md` (e.g. `skills/architecture/ABOUT.md`) |
| Cross-link format | `skills/category/skill-name` — no `@` prefix, no `/SKILL.md` suffix |
| Versioning | Bump frontmatter `version` (semver) on non-trivial edits |

## Implementation

### Required SKILL.md frontmatter

```yaml
---
name: Human-Readable Name
description: One-line summary
when_to_use: when [trigger/situation]
version: X.Y.Z
languages: all
---
```

Body sections, in order: Overview → When to Use → Quick Reference → Implementation → (optional) Common Mistakes / Real-World Impact. See `skills/meta/writing-skills` for the full template, token budgets, and CSO (Claude Search Optimization) rules.

### Adding a new skill

1. Search `skills/REQUESTS.md` to see if it's already requested or being discussed.
2. Pick a category directory. If none fits, propose a new category in the PR description rather than silently creating one.
3. Create `skills/<category>/<skill-name>/SKILL.md`. Match the shape of an existing peer in that category before inventing a new one.
4. Follow `skills/meta/writing-skills` (TDD for skills — RED/GREEN/REFACTOR with subagent pressure tests). Skipping this is a violation, not a shortcut.
5. If the skill is derived from external work, add a sibling `ABOUT.md` documenting the origin.

### Editing an existing skill

- Bump `version` in frontmatter (semver).
- Re-test with subagents per `skills/meta/testing-skills-with-subagents` if behavior changes.
- Don't rewrite cross-references to use `@` syntax — that force-loads files and burns context.

### Git workflow

- Feature branches per session; the current session's branch is `claude/init-project-setup-odj3Z`.
- PR-based contribution per `README.md`. Open PRs as **draft** by default.
- Commit messages: short imperative, optionally prefixed with `Fix:` / `Feat:` (see `git log --oneline` for examples).

## Common Mistakes

| Mistake | Why it matters |
|---------|---------------|
| Treating this repo like a code repo (looking for `package.json`, build commands) | There are none. The repo is documentation that runs in another agent's context. |
| Using `@skills/.../SKILL.md` to cross-reference | `@` force-loads the file immediately, costing 200k+ context. Use plain paths. |
| Editing a skill without bumping `version` | Downstream installs can't tell something changed. |
| Adding "project-specific" notes to a skill | Skills are reusable across projects. Project-specific guidance belongs in that project's CLAUDE.md, not here. |
| Creating a new category folder casually | Categories are the top-level taxonomy. Discuss in the PR before adding one. |
| Authoring a skill without baseline subagent testing | `skills/meta/writing-skills` Iron Law: no skill without a failing test first. Applies to edits too. |

## Why this file is at the repo root

Most skills live under `skills/<category>/`. This one is at the root because it's read **before** an agent has oriented enough to know the layout — it answers the "where am I and what do I do here?" question that has to be answered first. It deliberately defers authoring discipline to `skills/meta/writing-skills` rather than duplicating it.
