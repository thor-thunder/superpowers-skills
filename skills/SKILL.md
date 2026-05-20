# Superpowers Skills Index

Complete index of all 31 skills in the superpowers-skills library. Skills are proven workflows and decision frameworks for AI-assisted software development.

**Read the [main documentation](../CLAUDE.md) to understand project structure and mandatory workflows.**

---

## 🚀 Quick Navigation

- [Architecture (1)](#architecture) - System design patterns
- [Collaboration (11)](#collaboration) - Team workflows and code management
- [Debugging (4)](#debugging) - Bug finding and fixing
- [Meta (5)](#meta) - Skill management and contribution
- [Problem-Solving (6)](#problem-solving) - Creative techniques
- [Research (1)](#research) - Learning and understanding
- [Testing (3)](#testing) - Testing practices
- [Entry Point (1)](#entry-point) - Getting started

---

## Entry Point

### Getting Started with Skills
- **File:** [using-skills/SKILL.md](./using-skills/SKILL.md)
- **When:** Start of any conversation
- **What:** Mandatory workflows, search tool, brainstorming triggers
- **Version:** 4.0.2

---

## Architecture

### 1. Preserving Productive Tensions
- **File:** [architecture/preserving-productive-tensions/SKILL.md](./architecture/preserving-productive-tensions/SKILL.md)
- **When:** Oscillating between equally valid approaches that optimize for different priorities
- **What:** Recognize valuable disagreements, preserve multiple approaches instead of forcing premature resolution
- **Version:** 1.1.0

---

## Collaboration

Team workflows, code management, and partner coordination.

### 1. Brainstorming Ideas Into Designs
- **File:** [collaboration/brainstorming/SKILL.md](./collaboration/brainstorming/SKILL.md)
- **When:** Partner describes feature/project idea, before writing code or plans
- **What:** Interactive idea refinement using Socratic method to develop fully-formed designs
- **Version:** 2.2.0

### 2. Dispatching Parallel Agents
- **File:** [collaboration/dispatching-parallel-agents/SKILL.md](./collaboration/dispatching-parallel-agents/SKILL.md)
- **When:** Facing 3+ independent failures that can be investigated without shared state
- **What:** Use multiple Claude agents to investigate and fix independent problems concurrently
- **Version:** 1.1.0

### 3. Executing Plans
- **File:** [collaboration/executing-plans/SKILL.md](./collaboration/executing-plans/SKILL.md)
- **When:** Partner provides complete implementation plan to execute
- **What:** Execute detailed plans in batches with review checkpoints
- **Version:** 2.2.0

### 4. Finishing a Development Branch
- **File:** [collaboration/finishing-a-development-branch/SKILL.md](./collaboration/finishing-a-development-branch/SKILL.md)
- **When:** Implementation complete, all tests pass, need to integrate work
- **What:** Complete feature development with structured options for merge, PR, or cleanup
- **Version:** 1.1.0

### 5. Code Review Reception
- **File:** [collaboration/receiving-code-review/SKILL.md](./collaboration/receiving-code-review/SKILL.md)
- **When:** Receiving code review feedback, before implementing suggestions
- **What:** Receive and act on feedback with technical rigor, not blind implementation
- **Version:** 1.1.0

### 6. Remembering Conversations
- **File:** [collaboration/remembering-conversations/SKILL.md](./collaboration/remembering-conversations/SKILL.md)
- **When:** Partner mentions past discussions, debugging familiar issues, seeking context
- **What:** Search previous Claude Code conversations for facts, patterns, decisions using semantic/text search
- **Tools:** TypeScript indexing tool in [assets/remembering-conversations-tool/](./assets/remembering-conversations-tool/)
- **Version:** 1.1.0

### 7. Requesting Code Review
- **File:** [collaboration/requesting-code-review/SKILL.md](./collaboration/requesting-code-review/SKILL.md)
- **When:** Completing tasks, implementing features, before merging
- **What:** Dispatch code-reviewer subagent to verify work meets requirements
- **Version:** 1.1.0

### 8. Subagent-Driven Development
- **File:** [collaboration/subagent-driven-development/SKILL.md](./collaboration/subagent-driven-development/SKILL.md)
- **When:** Executing implementation plans with independent tasks in current session
- **What:** Execute plans by dispatching fresh subagent for each task, with code review gates
- **Version:** 1.1.0

### 9. Using Git Worktrees
- **File:** [collaboration/using-git-worktrees/SKILL.md](./collaboration/using-git-worktrees/SKILL.md)
- **When:** Starting feature work needing isolation from current workspace
- **What:** Create isolated git worktrees with smart directory selection and safety verification
- **Version:** 1.1.0

### 10. Writing Plans
- **File:** [collaboration/writing-plans/SKILL.md](./collaboration/writing-plans/SKILL.md)
- **When:** Design complete, need detailed implementation tasks for engineers with zero context
- **What:** Create detailed implementation plans with bite-sized tasks
- **Version:** 2.1.0

---

## Debugging

Frameworks for finding and fixing bugs systematically.

### 1. Defense-in-Depth Validation
- **File:** [debugging/defense-in-depth/SKILL.md](./debugging/defense-in-depth/SKILL.md)
- **When:** Invalid data causes failures deep in execution
- **What:** Validate at every layer to make bugs impossible
- **Version:** 1.1.0

### 2. Root Cause Tracing
- **File:** [debugging/root-cause-tracing/SKILL.md](./debugging/root-cause-tracing/SKILL.md)
- **When:** Errors occur deep in execution, need to find original trigger
- **What:** Systematically trace bugs backward through call stack
- **Tools:** Shell script in [assets/debugging-tools/](./assets/debugging-tools/)
- **Version:** 1.1.0

### 3. Systematic Debugging
- **File:** [debugging/systematic-debugging/SKILL.md](./debugging/systematic-debugging/SKILL.md)
- **When:** Encountering any bug, test failure, or unexpected behavior
- **What:** Four-phase debugging framework ensuring root cause before fixes
- **Version:** 2.1.0

### 4. Verification Before Completion
- **File:** [debugging/verification-before-completion/SKILL.md](./debugging/verification-before-completion/SKILL.md)
- **When:** About to claim work is complete, fixed, or passing
- **What:** Run verification commands and confirm output before success
- **Version:** 1.1.0

---

## Meta

Skills for managing, maintaining, and contributing to the skills library.

### 1. Gardening Skills Wiki
- **File:** [meta/gardening-skills-wiki/SKILL.md](./meta/gardening-skills-wiki/SKILL.md)
- **When:** Adding, removing, or reorganizing skills; periodic maintenance
- **What:** Maintain wiki health - check links, naming, cross-references, coverage
- **Tools:** Validation scripts in [assets/gardening-tools/](./assets/gardening-tools/)
- **Version:** 1.1.0

### 2. Pulling Updates from Skills Repository
- **File:** [meta/pulling-updates-from-skills-repository/SKILL.md](./meta/pulling-updates-from-skills-repository/SKILL.md)
- **When:** Syncing local skills with upstream changes
- **What:** Sync local skills repository with upstream from obra/superpowers-skills
- **Version:** 1.0.0

### 3. Sharing Skills
- **File:** [meta/sharing-skills/SKILL.md](./meta/sharing-skills/SKILL.md)
- **When:** Want to share your own skills with the community
- **What:** Documentation for sharing new skills
- **Version:** 1.0.0

### 4. Testing Skills with Subagents
- **File:** [meta/testing-skills-with-subagents/SKILL.md](./meta/testing-skills-with-subagents/SKILL.md)
- **When:** Writing or updating skills that need validation
- **What:** Test skill implementations using fresh subagents
- **Examples:** See [references/patterns/](./references/patterns/)
- **Version:** 1.0.0

### 5. Writing Skills
- **File:** [meta/writing-skills/SKILL.md](./meta/writing-skills/SKILL.md)
- **When:** Creating new skills to share with community
- **What:** Guidelines for writing high-quality, reusable skills
- **Conventions:** See [references/conventions/](./references/conventions/)
- **Version:** 1.1.0

---

## Problem-Solving

Creative techniques for tackling difficult problems.

### 1. Collision-Zone Thinking
- **File:** [problem-solving/collision-zone-thinking/SKILL.md](./problem-solving/collision-zone-thinking/SKILL.md)
- **When:** Facing incompatible constraints or competing requirements
- **What:** Identify collision zones and generate solutions that satisfy both constraints
- **Version:** 1.1.0

### 2. Inversion Exercise
- **File:** [problem-solving/inversion-exercise/SKILL.md](./problem-solving/inversion-exercise/SKILL.md)
- **When:** Problem feels stuck, direct approaches aren't working
- **What:** Reverse problem statement to uncover hidden assumptions and constraints
- **Version:** 1.1.0

### 3. Meta-Pattern Recognition
- **File:** [problem-solving/meta-pattern-recognition/SKILL.md](./problem-solving/meta-pattern-recognition/SKILL.md)
- **When:** Solving problems that feel connected to other domains
- **What:** Recognize abstract patterns across different problem domains
- **Version:** 1.1.0

### 4. Scale Game
- **File:** [problem-solving/scale-game/SKILL.md](./problem-solving/scale-game/SKILL.md)
- **When:** Problem feels intractable at current scale
- **What:** Change scale of problem to reveal hidden structure and solutions
- **Version:** 1.1.0

### 5. Simplification Cascades
- **File:** [problem-solving/simplification-cascades/SKILL.md](./problem-solving/simplification-cascades/SKILL.md)
- **When:** Complex problem with many interconnected pieces
- **What:** Systematically simplify to separate concerns and solve independently
- **Version:** 1.1.0

### 6. When Stuck
- **File:** [problem-solving/when-stuck/SKILL.md](./problem-solving/when-stuck/SKILL.md)
- **When:** No clear next steps, progress halted
- **What:** Systematic framework for unsticking without forcing
- **Version:** 1.1.0

---

## Research

Methods for learning and understanding complex topics.

### 1. Tracing Knowledge Lineages
- **File:** [research/tracing-knowledge-lineages/SKILL.md](./research/tracing-knowledge-lineages/SKILL.md)
- **When:** Learning new topic, understanding evolution of ideas
- **What:** Trace knowledge backward through sources and authors to understand foundations
- **Version:** 1.1.0

---

## Testing

Approaches and practices for testing code.

### 1. Condition-Based Waiting
- **File:** [testing/condition-based-waiting/SKILL.md](./testing/condition-based-waiting/SKILL.md)
- **When:** Tests need to wait for async operations or system state changes
- **What:** Wait for conditions rather than fixed durations for reliable async testing
- **Version:** 1.1.0

### 2. Test-Driven Development
- **File:** [testing/test-driven-development/SKILL.md](./testing/test-driven-development/SKILL.md)
- **When:** Starting new feature or test suite
- **What:** Write tests before implementation to guide design
- **Version:** 1.1.0

### 3. Testing Anti-Patterns
- **File:** [testing/testing-anti-patterns/SKILL.md](./testing/testing-anti-patterns/SKILL.md)
- **When:** Tests are flaky, hard to maintain, or not catching bugs
- **What:** Recognize and fix common testing pitfalls
- **Version:** 1.1.0

---

## Resources

- **[Shared Tools](./assets/)** - Centralized tools used across multiple skills
  - remembering-conversations-tool - Semantic search for past conversations
  - gardening-tools - Validation and maintenance scripts
  - debugging-tools - Debugging utilities
  
- **[References](./references/)** - Shared documentation and patterns
  - conventions/ - Graphviz and writing conventions
  - patterns/ - Testing examples and common patterns
  - attribution/ - Author credits and sources

---

## Contributing

Want to contribute? See [Writing Skills](./meta/writing-skills/SKILL.md) for guidelines.

Skills are community-driven. Every skill can be improved, and new skills are always welcome.

---

**Total Skills:** 31 | **Categories:** 7 | **Last Updated:** 2025  
**Repository:** thor-thunder/superpowers-skills | **License:** MIT
