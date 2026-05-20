# Shared Assets

Centralized location for tools, scripts, and utilities used across multiple skills.

## Contents

### remembering-conversations-tool/
TypeScript/Node.js tool for semantic search over Claude Code conversation history.

**Purpose:** Search previous conversations for facts, patterns, decisions, and context.

**Key Files:**
- `package.json` - Dependencies and scripts
- `src/embeddings.ts` - Embedding generation for semantic search
- `src/search.ts` - Core search functionality
- `src/search-cli.ts` - CLI interface for conversation search
- `src/db.ts` - SQLite database for conversation indexing
- `index-conversations` - Script to index existing conversations
- `search-conversations` - CLI tool to search indexed conversations
- `install-hook` - Installation hook for automatic indexing

**Related Skill:** [Remembering Conversations](../collaboration/remembering-conversations/SKILL.md)

**Documentation:**
- [Deployment Guide](../references/patterns/remembering-conversations-deployment.md)
- [Indexing Guide](../references/patterns/remembering-conversations-indexing.md)

### gardening-tools/
Shell scripts for maintaining skills library health and consistency.

**Purpose:** Validate skill structure, check links, verify naming conventions, and monitor coverage.

**Key Scripts:**
- `garden.sh` - Master script that runs all validations
- `check-links.sh` - Verify all markdown links are valid
- `check-naming.sh` - Verify skills follow naming conventions
- `check-index-coverage.sh` - Ensure all skills are in index
- `analyze-search-gaps.sh` - Find skills not easily discoverable

**Related Skill:** [Gardening Skills Wiki](../meta/gardening-skills-wiki/SKILL.md)

**Usage:**
```bash
./gardening-tools/garden.sh
```

### debugging-tools/
Utilities for debugging and tracing issues.

**Purpose:** Help find and trace bugs in complex systems.

**Key Tools:**
- `find-polluter.sh` - Trace backward through call stack to find bug origin

**Related Skill:** [Root Cause Tracing](../debugging/root-cause-tracing/SKILL.md)

---

## Using Assets

When a skill references a tool, it will link to these centralized locations:
- Tool source: `../assets/[tool-name]/`
- Installation: See skill documentation for setup instructions
- Configuration: Typically in `~/.config/superpowers/` or Claude Code settings

## Contributing

To add a new shared asset:

1. Implement the tool in a dedicated subdirectory
2. Include clear documentation in the subdirectory
3. Add reference to this README
4. Update related skill documentation to point to the asset
5. Commit and PR with description of what asset provides

---

**Last Updated:** 2025 | **License:** MIT
