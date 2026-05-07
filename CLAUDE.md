---
name: Working in the superpowers-skills repo
description: Orientation for agents editing the superpowers skills library — what this repo is, where things live, how to author skills, and what NOT to do
when_to_use: when starting a session inside the superpowers-skills repository, or about to add, edit, or move any skill in it
version: 0.2.0
languages: all
---

# Working in the superpowers-skills repo

## Overview

This repository **is** the source of truth for Claude Code's superpowers skills. The plugin clones it to `~/.config/superpowers/skills/`, where future agents read SKILL.md files at runtime. **Edits here ship as instructions executed by other agents** — wording is behavior, so treat changes with the care you'd give to code.

The repo contains only markdown. There is no build step, no test suite, no linter, no CI. Validation is by human review on PRs and by running pressure scenarios against subagents (see "Authoring discipline" below).

## When to Use

Read this file when:

- A session starts in the superpowers-skills repo
- You're about to create a new skill, edit an existing one, or restructure categories
- You're orienting before answering a question about repo conventions
- You hit "is there a build/test command?" — answer is below

## Quick Reference

| Topic | Answer |
|-------|--------|
| What is this repo? | The skills library that the superpowers plugin clones to `~/.config/superpowers/skills/` |
| Build / test / lint? | None. Markdown only. |
| Where do skills live? | `skills/<category>/<kebab-case-name>/SKILL.md` |
| Required filename | `SKILL.md` (uppercase, exactly) |
| Categories in use | architecture, collaboration, debugging, meta, problem-solving, research, testing, using-skills |
| New skill ideas | Check `skills/REQUESTS.md` first |
| Attribution for derived skills | Sibling `ABOUT.md` (e.g. `skills/architecture/ABOUT.md`) |
| Cross-link format | `skills/category/skill-name` — no `@` prefix, no `/SKILL.md` suffix |
| Versioning | Bump frontmatter `version` (semver) on non-trivial edits |

## Required SKILL.md frontmatter

```yaml
---
name: Human-Readable Name
description: One-line summary
when_to_use: when [trigger/situation]
version: X.Y.Z
languages: all
---
```

Body sections, in order: **Overview → When to Use → Quick Reference → Implementation → Common Mistakes** (optional Real-World Impact).

## Session-start workflow (the using-skills rules, inline)

These rules are mandatory for any agent editing in this repo OR using its skills downstream:

1. **Use Read tool before announcing skill usage.** The session-start hook does NOT read skills for you. Announcing without calling Read = lying.
2. **Follow mandatory workflows.** Brainstorming before coding. Check for skills before ANY task.
3. **Create TodoWrite todos for every checklist item** in a skill. Mental tracking = steps get skipped. Every time.
4. **If a skill for your task exists, you must use it** — read the entire file (not just frontmatter), announce "I've read [Skill Name] skill and I'm using it to [purpose]", and follow it exactly.
5. **Specific instructions ≠ permission to skip workflows.** "Add X" / "Fix Y" describes the goal, not a license to skip TDD or brainstorming. Specific instructions are exactly when workflows matter most.

## Authoring discipline (the writing-skills rules, inline)

**Writing a skill IS test-driven development applied to process documentation.** The Iron Law:

```
NO SKILL WITHOUT A FAILING TEST FIRST
```

This applies to NEW skills AND to EDITS of existing skills. No exceptions:

- Not for "simple additions"
- Not for "just adding a section"
- Not for "documentation updates"
- Don't keep untested changes as "reference"
- Don't "adapt" while running tests

### RED–GREEN–REFACTOR for skills

| Phase | What you do |
|-------|-------------|
| **RED** | Run a pressure scenario with a subagent **without** the skill. Document exact failures and rationalizations verbatim. |
| **GREEN** | Write the minimal skill that addresses those specific failures. Re-run the scenario; verify the agent now complies. |
| **REFACTOR** | Capture new rationalizations the agent finds. Add explicit counters, rationalization-table rows, red-flag entries. Re-test until bulletproof. |

### Pressure scenarios

Tests must combine **3+ pressures** to be meaningful. Pressure types:

| Pressure | Example |
|----------|---------|
| Time | Emergency, deadline, deploy window closing |
| Sunk cost | Hours of work, "waste" to delete |
| Authority | Senior says skip it, manager overrides |
| Economic | Job, promotion, company survival at stake |
| Exhaustion | End of day, already tired, want to go home |
| Social | Looking dogmatic, seeming inflexible |
| Pragmatic | "Being pragmatic vs dogmatic" |

A scenario is **good** if it forces an A/B/C choice with concrete options, real constraints, real file paths, and no easy outs ("I'd ask the human" doesn't count).

### When to create a skill

**Create when:** technique wasn't intuitively obvious; you'd reference it across projects; pattern is broadly applicable; others would benefit.

**Don't create for:** one-off solutions; standard practices documented elsewhere; project-specific conventions (those go in that project's CLAUDE.md, not here).

### Token efficiency targets

Skills load into other agents' contexts. Word budgets:

- `using-skills` and other entry-point workflows: **<150 words each**
- Frequently-loaded skills: **<200 words total**
- Other skills: **<500 words**

Move details to tool `--help`, compress examples, eliminate redundancy.

### Naming and CSO (Claude Search Optimization)

- Verb-first, active voice: `creating-skills`, not `skill-creation`
- Gerunds work well for processes: `creating-skills`, `testing-skills`, `debugging-with-logs`
- `when_to_use` must start with "when" so it completes "Use [skill] when [trigger]"
- `when_to_use` describes the **problem** (race conditions, inconsistent behavior), not language-specific symptoms (`setTimeout`, `sleep`)
- Repeat key concepts in description, when_to_use, overview, and section headers — multiple grep hits = easier discovery

### Cross-references

Use plain paths: `skills/testing/test-driven-development`. Never use `@skills/testing/test-driven-development/SKILL.md` — the `@` syntax force-loads the file and burns 200k+ context before anyone needs it.

To actually read a referenced skill, use the Read tool on `${SUPERPOWERS_SKILLS_ROOT}/category/skill-name/SKILL.md`.

## Adding a new skill

1. Search `skills/REQUESTS.md` to see if it's already requested or being discussed.
2. Pick a category directory. If none fits, propose a new category in the PR description rather than silently creating one.
3. Run a **baseline pressure scenario** with a subagent and document its rationalizations (RED phase above).
4. Create `skills/<category>/<skill-name>/SKILL.md` using the frontmatter template, addressing the specific failures you observed (GREEN phase).
5. Match the shape of an existing peer in that category before inventing a new one.
6. Iterate against new rationalizations until bulletproof (REFACTOR phase).
7. If the skill is derived from external work, add a sibling `ABOUT.md` documenting the origin.

## Editing an existing skill

- Bump `version` in frontmatter (semver) on any non-trivial change.
- Re-run pressure scenarios — same Iron Law applies to edits as to new skills.
- Don't introduce `@` cross-references.

## Git workflow

- Feature branches per session; the current session's branch is `claude/init-project-setup-odj3Z`. Develop, commit, and push there.
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
| Authoring or editing a skill without baseline subagent testing | Iron Law: no skill without a failing test first. Applies to edits too. |
| Writing skills as narratives ("In session 2025-10-03 we found...") | Skills are reusable references, not war stories. Strip the narrative. |
| Multi-language example dilution | One excellent example beats five mediocre ones. Pick the most relevant language. |
