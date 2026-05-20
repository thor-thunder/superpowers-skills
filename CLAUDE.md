# Superpowers Skills Library

A community-editable skill library for Claude Code's superpowers plugin. This repository contains **31 proven skills** organized into 7 categories for AI-assisted software development.

## What Are Skills?

Skills are reusable workflows and decision frameworks derived from the Amplifier agent research patterns. They encode proven techniques for brainstorming, planning, debugging, collaboration, and problem-solving.

**Key principle:** If a skill exists for your task, you must use it. Skills document proven techniques that save time and prevent mistakes.

## Quick Start

1. **Find relevant skills** for your task:
   - Browse the [Skills Index](./skills/SKILL.md) for complete list
   - Or search: `find-skills [PATTERN]`

2. **Read the skill** before using:
   ```bash
   Read: ${SUPERPOWERS_SKILLS_ROOT}/skills/category/skill-name/SKILL.md
   ```

3. **Follow the skill exactly** - skills evolve, so always read current version

4. **Create TodoWrite todos** if skill has a checklist - don't track mentally

## Project Structure

```
superpowers-skills/
├── CLAUDE.md                  ← Project documentation (you are here)
├── README.md                  ← Repository overview
├── LICENSE                    ← MIT License
├── skills/
│   ├── SKILL.md              ← Complete skills index and navigation
│   ├── using-skills/         ← Getting started with skills
│   ├── architecture/         ← Architectural patterns (1 skill)
│   ├── collaboration/        ← Team collaboration workflows (11 skills)
│   ├── debugging/            ← Debugging frameworks (4 skills)
│   ├── meta/                 ← Skill management & contribution (5 skills)
│   ├── problem-solving/      ← Creative problem-solving (6 skills)
│   ├── research/             ← Research methods (1 skill)
│   ├── testing/              ── Testing practices (3 skills)
│   ├── assets/               ← Centralized shared tools and utilities
│   └── references/           ← Shared documentation and patterns
```

## Skill Categories

### 🏛️ Architecture (1 skill)
Patterns for designing systems and managing trade-offs:
- **Preserving Productive Tensions** - Recognize valuable disagreements, preserve multiple valid approaches

### 🤝 Collaboration (11 skills)
Workflows for working with partners and managing code:
- Brainstorming Ideas Into Designs
- Dispatching Parallel Agents
- Executing Plans
- Finishing a Development Branch
- Code Review Reception
- Remembering Conversations
- Requesting Code Review
- Subagent-Driven Development
- Using Git Worktrees
- Writing Plans

### 🐛 Debugging (4 skills)
Frameworks for finding and fixing bugs:
- Defense-in-Depth Validation
- Root Cause Tracing
- Systematic Debugging
- Verification Before Completion

### 📚 Meta (5 skills)
Skills for contributing to and maintaining the skills library:
- Gardening Skills Wiki
- Pulling Updates from Skills Repository
- Sharing Skills
- Testing Skills with Subagents
- Writing Skills

### 🧩 Problem-Solving (6 skills)
Creative techniques for difficult problems:
- Collision-Zone Thinking
- Inversion Exercise
- Meta-Pattern Recognition
- Scale Game
- Simplification Cascades
- When Stuck

### 🔍 Research (1 skill)
Methods for understanding and learning:
- Tracing Knowledge Lineages

### ✅ Testing (3 skills)
Approaches for testing and validation:
- Condition-Based Waiting
- Test-Driven Development
- Testing Anti-Patterns

## Mandatory Workflows

1. **Check for Skills** - At the start of any task, check if a relevant skill exists
2. **Brainstorm Before Code** - Use the Brainstorming skill before implementing features
3. **Read Current Version** - Always read skill files; don't rely on memory
4. **Create TodoWrite Todos** - If a skill has a checklist, create todos for each item

## For Contributors

- **Adding a Skill:** See [Writing Skills](./skills/meta/writing-skills/SKILL.md)
- **Updating Skills:** See [Gardening Skills Wiki](./skills/meta/gardening-skills-wiki/SKILL.md)
- **Contributing:** Fork, create a branch, add skills, submit PR
- **License:** All contributions are MIT licensed

## Resources

- **Tools & Utilities:** See [assets/](./skills/assets/) for shared tools like the remembering-conversations indexing tool
- **Patterns & Examples:** See [references/](./skills/references/) for documentation and learning materials
- **Complete Index:** See [skills/SKILL.md](./skills/SKILL.md) for links to all individual skills

## Key Tools

- **Remembering Conversations Tool** - Semantic search over past Claude Code conversations using embeddings
- **Gardening Scripts** - Automated validation of skill structure, links, and naming conventions

## Philosophy

Skills are:
- **Proven** - Tested and refined through real development work
- **Complete** - Include decision trees, checklists, and edge cases
- **Focused** - Single responsibility, specific use cases
- **Learnable** - Written for humans, not just machines
- **Evolvable** - Community contributions improve them continuously

## Getting Help

- Questions about using skills? Read [Getting Started with Skills](./skills/using-skills/SKILL.md)
- Want to contribute? Check the [Contributing Guide](./skills/meta/writing-skills/SKILL.md)
- Found a bug in a skill? Open an issue describing the problem
- Want to share your own skill? See [Sharing Skills](./skills/meta/sharing-skills/SKILL.md)

---

**Version:** 1.0.0 | **Updated:** 2025  
**Repository:** thor-thunder/superpowers-skills  
**License:** MIT
