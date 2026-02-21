# Generate Codebase Snapshot

You are creating a persistent codebase snapshot so that future Claude Code sessions can load context instantly instead of re-exploring from scratch.

## Instructions

### Step 1: Identify the project

- Determine the current working directory and project type (language, framework, monorepo vs single package)
- Get the current git commit hash with `git log --oneline -1`
- Read any existing CLAUDE.md or README.md for project context

### Step 2: Explore the codebase thoroughly

Use Explore agents (or Glob/Grep/Read) to build a complete understanding of:

1. **Directory tree** — Every source file with path, line count, and 1-line description
2. **Architecture** — How the codebase is structured, key directories, entry points
3. **Data models** — Database schemas, type definitions, interfaces
4. **API surface** — All endpoints/routes/exported functions with method + path + description
5. **State management** — Stores, contexts, reducers — state fields and actions
6. **Key patterns** — Error handling, auth, validation, testing patterns used consistently
7. **Configuration** — Build tools, environment variables, deployment setup
8. **Dependencies** — Key packages and what they're used for

For large codebases, run multiple Explore agents in parallel (e.g., one for backend, one for frontend, one for config/infra).

### Step 3: Write the snapshot

Write a file called `codebase-snapshot.md` in your persistent memory directory with ALL of the above information structured for quick reference. Include at the top:

```
# Codebase Snapshot — [Project Name]

**Snapshot Commit:** `[hash]` ([commit message])
**Generated:** [date]
```

The snapshot should be comprehensive enough that a new Claude session can understand the entire codebase without reading any source files. Aim for 300-600 lines depending on project size.

### Step 4: Write/update MEMORY.md

Write or update `MEMORY.md` in your persistent memory directory with these instructions for future sessions:

```markdown
# [Project Name] — Claude Context Cache

## Quick Start for New Sessions

**DO NOT use Explore agents to understand the codebase from scratch.**
Instead, follow this 2-step process:

### Step 1: Load the snapshot
Read `codebase-snapshot.md` in this directory. It contains the complete codebase understanding.

### Step 2: Check what changed since the snapshot
Run: `git log --oneline [SNAPSHOT_COMMIT]..HEAD`

If there are changes, run: `git diff --stat [SNAPSHOT_COMMIT]..HEAD` to see which files changed.
Then read ONLY the changed files to update your understanding.

**Current snapshot commit:** `[hash]`

## When to Update the Snapshot
Update when the diff from snapshot grows large (>15 files changed) or when major structural changes occur. Run `/update-snapshot` to refresh.
```

Add any project-specific conventions or user preferences below the quick start section.

### Step 5: Report results

Tell the user:
- What was snapshotted (file count, line count, key sections)
- The git commit hash it was pinned to
- How to use it in future sessions (it's automatic — MEMORY.md is auto-loaded)
- How to update it later (`/update-snapshot`)

$ARGUMENTS
