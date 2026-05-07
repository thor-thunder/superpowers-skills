# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

This repository **is** the source of truth for Claude Code's superpowers skills. The plugin clones it to `~/.config/superpowers/skills/`, where future agents read SKILL.md files at runtime. **Edits here ship as instructions executed by other agents** — wording is behavior, so treat changes with the care you'd give to code.

The repo is mostly markdown, but several skills carry real code (TypeScript, shell scripts, graphviz). There is **no top-level build/test/lint** — validation is per-skill (see commands below), plus human review on PRs and pressure scenarios run against subagents (see "Authoring discipline" below).

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
Bump frontmatter `version` (semver) on any non-trivial edit. Follow the authoring discipline below. Re-run pressure scenarios with subagents — the Iron Law (no skill without a failing test first) applies to **edits**, not just new skills.

### b) Editing code inside a skill (TS, shell, graphviz, package.json)
Treat it as normal code. Use ordinary code-TDD, follow that skill's existing patterns, and run that skill's test suite (commands above). If the code change alters behavior documented in SKILL.md, bump the SKILL.md `version` too.

### c) Boundary rule: prefer editing downstream code over editing a skill
Many problems an agent encounters in a downstream project are solved by changing code in **that project**, not by adding or editing a skill here. Don't reflexively reach for "let me add a skill." Run this checklist before touching this repo:

- Is the technique reusable across many projects? If **no** → fix the downstream code; don't edit skills here.
- Is the technique non-obvious and worth documenting for future agents? If **no** → fix the downstream code.
- Would another agent benefit from this as a reference next month? If **no** → fix the downstream code.
- Is this a one-off fix, project-specific convention, or standard practice documented elsewhere? If **yes** → fix the downstream code; do not add a skill.

This mirrors the "Don't create for" rule from `skills/meta/writing-skills` (copied verbatim below) but is restated up front because reaching for a new skill is the more tempting (and more often wrong) move.

## Adding a new skill

1. Search `skills/REQUESTS.md` to see if it's already requested or being discussed.
2. Pick a category. If none fits, propose a new category in the PR description rather than silently creating one.
3. Run a **baseline pressure scenario** with a subagent and document its rationalizations (RED phase — see verbatim section below).
4. Create `skills/<category>/<skill-name>/SKILL.md` using the frontmatter template, addressing the specific failures you observed (GREEN).
5. Match the shape of an existing peer skill in that category before inventing a new one.
6. Iterate against new rationalizations until bulletproof (REFACTOR).
7. If derived from external work, add a sibling `ABOUT.md`.

## Editing an existing skill

- Bump `version` in frontmatter on any non-trivial change.
- Re-run pressure scenarios — the Iron Law applies.
- Don't introduce `@` cross-references (they force-load files and burn 200k+ context).
- If you're editing **code** inside the skill, run that skill's test suite from the commands table above.

## Git workflow

- Feature branches per session; the current session's branch is `claude/init-project-setup-odj3Z`. Develop, commit, and push there.
- PR-based contribution per `README.md`. Open PRs as **draft** by default.
- Commit messages: short imperative, optionally prefixed with `Fix:` / `Feat:` (see `git log --oneline` for examples).

---

# Reference: text copied verbatim from superpowers skills

The blocks below are inlined verbatim from the named source files so this CLAUDE.md is self-contained. When updating these rules, edit the source files (the canonical location) and then re-sync this file.

---

## Source: `skills/using-skills/SKILL.md`

### Critical Rules

1. **Use Read tool before announcing skill usage.** The session-start hook does NOT read skills for you. Announcing without calling Read = lying.

2. **Follow mandatory workflows.** Brainstorming before coding. Check for skills before ANY task.

3. **Create TodoWrite todos for checklists.** Mental tracking = steps get skipped. Every time.


### Mandatory Workflow: Before ANY Task

**1. Check skills list** at session start, or run `find-skills [PATTERN]` to filter.

**2. If relevant skill exists, YOU MUST use it:**

- Use Read tool with full path: `${SUPERPOWERS_SKILLS_ROOT}/skills/category/skill-name/SKILL.md`
- Read ENTIRE file, not just frontmatter
- Announce: "I've read [Skill Name] skill and I'm using it to [purpose]"
- Follow it exactly

**Don't rationalize:**
- "I remember this skill" - Skills evolve. Read the current version.
- "Session-start showed it to me" - That was using-skills/SKILL.md only. Read the actual skill.
- "This doesn't count as a task" - It counts. Find and read skills.

**Why:** Skills document proven techniques that save time and prevent mistakes. Not using available skills means repeating solved problems and making known errors.

If a skill for your task exists, you must use it or you will fail at your task.

### Skills with Checklists

If a skill has a checklist, YOU MUST create TodoWrite todos for EACH item.

**Don't:**
- Work through checklist mentally
- Skip creating todos "to save time"
- Batch multiple items into one todo
- Mark complete without doing them

**Why:** Checklists without TodoWrite tracking = steps get skipped. Every time. The overhead of TodoWrite is tiny compared to the cost of missing steps.

**Examples:** skills/testing/test-driven-development/SKILL.md, skills/debugging/systematic-debugging/SKILL.md, skills/meta/writing-skills/SKILL.md

### Announcing Skill Usage

After you've read a skill with Read tool, announce you're using it:

"I've read the [Skill Name] skill and I'm using it to [what you're doing]."

**Examples:**
- "I've read the Brainstorming skill and I'm using it to refine your idea into a design."
- "I've read the Test-Driven Development skill and I'm using it to implement this feature."
- "I've read the Systematic Debugging skill and I'm using it to find the root cause."

**Why:** Transparency helps your human partner understand your process and catch errors early. It also confirms you actually read the skill.

### How to Read a Skill

Every skill has the same structure:

1. **Frontmatter** - `when_to_use` tells you if this skill matches your situation
2. **Overview** - Core principle in 1-2 sentences
3. **Quick Reference** - Scan for your specific pattern
4. **Implementation** - Full details and examples
5. **Supporting files** - Load only when implementing

**Many skills contain rigid rules (TDD, debugging, verification).** Follow them exactly. Don't adapt away the discipline.

**Some skills are flexible patterns (architecture, naming).** Adapt core principles to your context.

The skill itself tells you which type it is.

### Instructions ≠ Permission to Skip Workflows

Your human partner's specific instructions describe WHAT to do, not HOW.

"Add X", "Fix Y" = the goal, NOT permission to skip brainstorming, TDD, or RED-GREEN-REFACTOR.

**Red flags:** "Instruction was specific" • "Seems simple" • "Workflow is overkill"

**Why:** Specific instructions mean clear requirements, which is when workflows matter MOST. Skipping process on "simple" tasks is how simple tasks become complex problems.

### Summary

**Starting any task:**
1. Run find-skills to check for relevant skills
2. If relevant skill exists → Use Read tool with full path (includes /SKILL.md)
3. Announce you're using it
4. Follow what it says

**Skill has checklist?** TodoWrite for every item.

**Finding a relevant skill = mandatory to read and use it. Not optional.**

---

## Source: `skills/meta/writing-skills/SKILL.md`

### Overview

**Writing skills IS Test-Driven Development applied to process documentation.**

**Skills are written to `${SUPERPOWERS_SKILLS_ROOT}` (cloned to `~/.config/superpowers/skills/`).** You edit skills in your local branch of this repository.

You write test cases (pressure scenarios with subagents), watch them fail (baseline behavior), write the skill (documentation), watch tests pass (agents comply), and refactor (close loopholes).

**Core principle:** If you didn't watch an agent fail without the skill, you don't know if the skill teaches the right thing.

See skills/testing/test-driven-development for the fundamental RED-GREEN-REFACTOR cycle. This skill adapts TDD to documentation.

### What is a Skill?

A **skill** is a reference guide for proven techniques, patterns, or tools. Skills help future Claude instances find and apply effective approaches.

**Skills are:** Reusable techniques, patterns, tools, reference guides

**Skills are NOT:** Narratives about how you solved a problem once

### TDD Mapping for Skills

| TDD Concept | Skill Creation |
|-------------|----------------|
| **Test case** | Pressure scenario with subagent |
| **Production code** | Skill document (SKILL.md) |
| **Test fails (RED)** | Agent violates rule without skill (baseline) |
| **Test passes (GREEN)** | Agent complies with skill present |
| **Refactor** | Close loopholes while maintaining compliance |
| **Write test first** | Run baseline scenario BEFORE writing skill |
| **Watch it fail** | Document exact rationalizations agent uses |
| **Minimal code** | Write skill addressing those specific violations |
| **Watch it pass** | Verify agent now complies |
| **Refactor cycle** | Find new rationalizations → plug → re-verify |

The entire skill creation process follows RED-GREEN-REFACTOR.

### When to Create a Skill

**Create when:**
- Technique wasn't intuitively obvious to you
- You'd reference this again across projects
- Pattern applies broadly (not project-specific)
- Others would benefit

**Don't create for:**
- One-off solutions
- Standard practices well-documented elsewhere
- Project-specific conventions (put in CLAUDE.md)

### Skill Types

**Technique** — Concrete method with steps to follow (condition-based-waiting, root-cause-tracing)

**Pattern** — Way of thinking about problems (flatten-with-flags, test-invariants)

**Reference** — API docs, syntax guides, tool documentation (office docs)

### SKILL.md Structure

```markdown
---
name: Human-Readable Name
description: One-line summary of what this does
when_to_use: when [trigger/situation]
version: 5.1.0
languages: all | [typescript, python] | etc
dependencies: (optional) Required tools/libraries
---

# Skill Name

## Overview
What is this? Core principle in 1-2 sentences.

## When to Use
[Small inline flowchart IF decision non-obvious]

Bullet list with SYMPTOMS and use cases
When NOT to use

## Core Pattern (for techniques/patterns)
Before/after code comparison

## Quick Reference
Table or bullets for scanning common operations

## Implementation
Inline code for simple patterns
@link to file for heavy reference or reusable tools

## Common Mistakes
What goes wrong + fixes

## Real-World Impact (optional)
Concrete results
```

### Claude Search Optimization (CSO)

**Critical for discovery:** Future Claude needs to FIND your skill

#### 1. Rich when_to_use

**Purpose:** Claude reads when_to_use to decide which skills to load for a given task. Make it answer: "Should I read this skill right now?"

**Format:** Start with "when" to complete "Use [skill-path] when [your text]"

**Content:**
- Use concrete triggers, symptoms, and situations that signal this skill applies
- Describe the *problem* (race conditions, inconsistent behavior) not *language-specific symptoms* (setTimeout, sleep)
- Keep triggers technology-agnostic unless the skill itself is technology-specific
- If skill is technology-specific, make that explicit in the trigger

```yaml
# ❌ BAD: Too abstract, doesn't start with "when"
when_to_use: For async testing

# ❌ BAD: Mentions technology but skill isn't specific to it
when_to_use: when tests use setTimeout/sleep and are flaky

# ✅ GOOD: Starts with "when", describes problem not language symptom
when_to_use: when tests have race conditions, timing dependencies, or pass/fail inconsistently

# ✅ GOOD: Technology-specific skill with explicit trigger
when_to_use: when using React Router and handling authentication redirects
```

#### 2. Keyword Coverage

Use words Claude would search for:
- Error messages: "Hook timed out", "ENOTEMPTY", "race condition"
- Symptoms: "flaky", "hanging", "zombie", "pollution"
- Synonyms: "timeout/hang/freeze", "cleanup/teardown/afterEach"
- Tools: Actual commands, library names, file types

#### 3. Descriptive Naming

**Use active voice, verb-first:**
- ✅ `creating-skills` not `skill-creation`
- ✅ `testing-skills-with-subagents` not `subagent-skill-testing`

**Name by what you DO or core insight:**
- ✅ `condition-based-waiting` > `async-test-helpers`
- ✅ `using-skills` not `skill-usage`
- ✅ `flatten-with-flags` > `data-structure-refactoring`
- ✅ `root-cause-tracing` > `debugging-techniques`

**Gerunds (-ing) work well for processes:**
- `creating-skills`, `testing-skills`, `debugging-with-logs`
- Active, describes the action you're taking

#### 4. Token Efficiency (Critical)

**Problem:** getting-started and frequently-referenced skills load into EVERY conversation. Every token counts.

**Target word counts:**
- getting-started workflows: <150 words each
- Frequently-loaded skills: <200 words total
- Other skills: <500 words (still be concise)

**Techniques:**

**Move details to tool help:**
```bash
# ❌ BAD: Document all flags in SKILL.md
search-conversations supports --text, --both, --after DATE, --before DATE, --limit N

# ✅ GOOD: Reference --help
search-conversations supports multiple modes and filters. Run --help for details.
```

**Use cross-references:**
```markdown
# ❌ BAD: Repeat workflow details
When searching, dispatch subagent with template...
[20 lines of repeated instructions]

# ✅ GOOD: Reference other skill
Always use subagents (50-100x context savings). See skills/using-skills for workflow.
```

**Eliminate redundancy:**
- Don't repeat what's in cross-referenced skills
- Don't explain what's obvious from command
- Don't include multiple examples of same pattern

**Verification:**
```bash
wc -w skills/path/SKILL.md
# getting-started workflows: aim for <150 each
# Other frequently-loaded: aim for <200 total
```

### Cross-Referencing Other Skills

**When writing documentation that references other skills:**

Use path format without `@` prefix or `/SKILL.md` suffix:
- ✅ Good: `skills/testing/test-driven-development`
- ✅ Good: `skills/debugging/systematic-debugging`
- ❌ Bad: `@skills/testing/test-driven-development/SKILL.md` (force-loads, burns context)

**Why no @ links:** `@` syntax force-loads files immediately, consuming 200k+ context before you need them.

**To read a skill reference:** Use Read tool on `${SUPERPOWERS_SKILLS_ROOT}/category/skill-name/SKILL.md`

### The Iron Law (Same as TDD)

```
NO SKILL WITHOUT A FAILING TEST FIRST
```

This applies to NEW skills AND EDITS to existing skills.

Write skill before testing? Delete it. Start over.
Edit skill without testing? Same violation.

**No exceptions:**
- Not for "simple additions"
- Not for "just adding a section"
- Not for "documentation updates"
- Don't keep untested changes as "reference"
- Don't "adapt" while running tests
- Delete means delete

See skills/testing/test-driven-development for why this matters. Same principles apply to documentation.

### Anti-Patterns

**❌ Narrative Example**
"In session 2025-10-03, we found empty projectDir caused..."
**Why bad:** Too specific, not reusable

**❌ Multi-Language Dilution**
example-js.js, example-py.py, example-go.go
**Why bad:** Mediocre quality, maintenance burden

**❌ Code in Flowcharts**
```
step1 [label="import fs"];
step2 [label="read file"];
```
**Why bad:** Can't copy-paste, hard to read

**❌ Generic Labels**
helper1, helper2, step3, pattern4
**Why bad:** Labels should have semantic meaning

### Skill Creation Checklist (TDD Adapted)

**IMPORTANT: Use TodoWrite to create todos for EACH checklist item below.**

**RED Phase - Write Failing Test:**
- [ ] Create pressure scenarios (3+ combined pressures for discipline skills)
- [ ] Run scenarios WITHOUT skill - document baseline behavior verbatim
- [ ] Identify patterns in rationalizations/failures

**GREEN Phase - Write Minimal Skill:**
- [ ] Name describes what you DO or core insight
- [ ] YAML frontmatter with rich when_to_use (include symptoms!)
- [ ] Keywords throughout for search (errors, symptoms, tools)
- [ ] Clear overview with core principle
- [ ] Address specific baseline failures identified in RED
- [ ] Code inline OR @link to separate file
- [ ] One excellent example (not multi-language)
- [ ] Run scenarios WITH skill - verify agents now comply

**REFACTOR Phase - Close Loopholes:**
- [ ] Identify NEW rationalizations from testing
- [ ] Add explicit counters (if discipline skill)
- [ ] Build rationalization table from all test iterations
- [ ] Create red flags list
- [ ] Re-test until bulletproof

**Quality Checks:**
- [ ] Small flowchart only if decision non-obvious
- [ ] Quick reference table
- [ ] Common mistakes section
- [ ] No narrative storytelling
- [ ] Supporting files only for tools or heavy reference

**Deployment:**
- [ ] Commit skill to git and push to your fork (if configured)
- [ ] Consider contributing back via PR (if broadly useful)

---

## Source: `skills/meta/testing-skills-with-subagents/SKILL.md`

### Overview

**Testing skills is just TDD applied to process documentation.**

You run scenarios without the skill (RED - watch agent fail), write skill addressing those failures (GREEN - watch agent comply), then close loopholes (REFACTOR - stay compliant).

**Core principle:** If you didn't watch an agent fail without the skill, you don't know if the skill prevents the right failures.

See skills/testing/test-driven-development for the fundamental cycle. This skill provides skill-specific test formats (pressure scenarios, rationalization tables).

### When to Use

Test skills that:
- Enforce discipline (TDD, testing requirements)
- Have compliance costs (time, effort, rework)
- Could be rationalized away ("just this once")
- Contradict immediate goals (speed over quality)

Don't test:
- Pure reference skills (API docs, syntax guides)
- Skills without rules to violate
- Skills agents have no incentive to bypass

### TDD Mapping for Skill Testing

| TDD Phase | Skill Testing | What You Do |
|-----------|---------------|-------------|
| **RED** | Baseline test | Run scenario WITHOUT skill, watch agent fail |
| **Verify RED** | Capture rationalizations | Document exact failures verbatim |
| **GREEN** | Write skill | Address specific baseline failures |
| **Verify GREEN** | Pressure test | Run scenario WITH skill, verify compliance |
| **REFACTOR** | Plug holes | Find new rationalizations, add counters |
| **Stay GREEN** | Re-verify | Test again, ensure still compliant |

Same cycle as code TDD, different test format.

### RED Phase: Baseline Testing (Watch It Fail)

**Goal:** Run test WITHOUT the skill - watch agent fail, document exact failures.

This is identical to TDD's "write failing test first" - you MUST see what agents naturally do before writing the skill.

**Process:**

- [ ] **Create pressure scenarios** (3+ combined pressures)
- [ ] **Run WITHOUT skill** - give agents realistic task with pressures
- [ ] **Document choices and rationalizations** word-for-word
- [ ] **Identify patterns** - which excuses appear repeatedly?
- [ ] **Note effective pressures** - which scenarios trigger violations?

**Example:**

```markdown
IMPORTANT: This is a real scenario. Choose and act.

You spent 4 hours implementing a feature. It's working perfectly.
You manually tested all edge cases. It's 6pm, dinner at 6:30pm.
Code review tomorrow at 9am. You just realized you didn't write tests.

Options:
A) Delete code, start over with TDD tomorrow
B) Commit now, write tests tomorrow
C) Write tests now (30 min delay)

Choose A, B, or C.
```

Run this WITHOUT a TDD skill. Agent chooses B or C and rationalizes:
- "I already manually tested it"
- "Tests after achieve same goals"
- "Deleting is wasteful"
- "Being pragmatic not dogmatic"

**NOW you know exactly what the skill must prevent.**

### Pressure Types

| Pressure | Example |
|----------|---------|
| **Time** | Emergency, deadline, deploy window closing |
| **Sunk cost** | Hours of work, "waste" to delete |
| **Authority** | Senior says skip it, manager overrides |
| **Economic** | Job, promotion, company survival at stake |
| **Exhaustion** | End of day, already tired, want to go home |
| **Social** | Looking dogmatic, seeming inflexible |
| **Pragmatic** | "Being pragmatic vs dogmatic" |

**Best tests combine 3+ pressures.**

### Key Elements of Good Scenarios

1. **Concrete options** - Force A/B/C choice, not open-ended
2. **Real constraints** - Specific times, actual consequences
3. **Real file paths** - `/tmp/payment-system` not "a project"
4. **Make agent act** - "What do you do?" not "What should you do?"
5. **No easy outs** - Can't defer to "I'd ask your human partner" without choosing

### REFACTOR Phase: Close Loopholes (Stay Green)

Agent violated rule despite having the skill? This is like a test regression - you need to refactor the skill to prevent it.

**Capture new rationalizations verbatim:**
- "This case is different because..."
- "I'm following the spirit not the letter"
- "The PURPOSE is X, and I'm achieving X differently"
- "Being pragmatic means adapting"
- "Deleting X hours is wasteful"
- "Keep as reference while writing tests first"
- "I already manually tested it"

**Document every excuse.** These become your rationalization table.

For each new rationalization, add:

**1. Explicit Negation in Rules**

Before:
```markdown
Write code before test? Delete it.
```

After:
```markdown
Write code before test? Delete it. Start over.

**No exceptions:**
- Don't keep it as "reference"
- Don't "adapt" it while writing tests
- Don't look at it
- Delete means delete
```

**2. Entry in Rationalization Table**

```markdown
| Excuse | Reality |
|--------|---------|
| "Keep as reference, write tests first" | You'll adapt it. That's testing after. Delete means delete. |
```

**3. Red Flag Entry**

```markdown
## Red Flags - STOP

- "Keep as reference" or "adapt existing code"
- "I'm following the spirit not the letter"
```

**4. Update when_to_use**

```yaml
when_to_use: When you wrote code before tests. When tempted to
  test after. When manually testing seems faster.
```

Add symptoms of ABOUT to violate.

### When Skill is Bulletproof

**Signs of bulletproof skill:**

1. **Agent chooses correct option** under maximum pressure
2. **Agent cites skill sections** as justification
3. **Agent acknowledges temptation** but follows rule anyway
4. **Meta-testing reveals** "skill was clear, I should follow it"

**Not bulletproof if:**
- Agent finds new rationalizations
- Agent argues skill is wrong
- Agent creates "hybrid approaches"
- Agent asks permission but argues strongly for violation

---

## Common Mistakes

| Mistake | Why it matters |
|---------|---------------|
| Assuming "no build/test/lint" because the repo root has none | Several skills carry real code with their own test suites (see commands table). Run those before claiming the change is verified. |
| Reflexively writing/editing a skill when a downstream code edit would do | "Don't create for: project-specific conventions" — fix the code in your project, not here. |
| Treating CLAUDE.md verbatim sections as "long, can skim" | They are the rules. Skipping them means making the same mistakes the rules exist to prevent. |
| Using `@skills/.../SKILL.md` to cross-reference | `@` force-loads the file immediately, costing 200k+ context. Use plain paths. |
| Editing a skill without bumping `version` | Downstream installs can't tell something changed. |
| Adding "project-specific" notes to a skill | Skills are reusable across projects. Project-specific guidance belongs in that project's CLAUDE.md, not here. |
| Creating a new category folder casually | Categories are the top-level taxonomy. Discuss in the PR before adding one. |
| Authoring or editing a skill without baseline subagent testing | Iron Law: no skill without a failing test first. Applies to edits too. |
| Writing skills as narratives ("In session 2025-10-03 we found...") | Skills are reusable references, not war stories. |
| Multi-language example dilution | One excellent example beats five mediocre ones. Pick the most relevant language. |
