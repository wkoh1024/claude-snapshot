# claude-snapshot

Slash commands for Claude Code that cache your entire codebase understanding so new sessions load instantly instead of re-exploring from scratch.

## The Problem

Every time you start a new Claude Code conversation, Claude needs to explore your codebase to understand it — reading files, running Explore agents, burning through tokens and time. For large projects, this can take minutes and hundreds of thousands of tokens.

## The Solution

**Snapshot + Diff.** Generate a comprehensive codebase snapshot once, then future sessions just read the snapshot and check `git diff` for changes. Think of it like database snapshots + WAL logs.

- `/generate-snapshot` — Full codebase exploration → writes a structured snapshot to Claude's persistent memory
- `/update-snapshot` — Reads only what changed since the last snapshot → patches the cached version

**Before:** ~200K+ tokens and 2-5 minutes to re-explore each session
**After:** ~5K tokens and a few seconds to read the snapshot + diff

## Install

Copy the command files to your Claude Code commands directory:

```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/claude-snapshot.git

# Copy commands to your global Claude commands directory
mkdir -p ~/.claude/commands
cp claude-snapshot/commands/*.md ~/.claude/commands/
```

Or just download the two files directly:

```bash
mkdir -p ~/.claude/commands
curl -o ~/.claude/commands/generate-snapshot.md https://raw.githubusercontent.com/YOUR_USERNAME/claude-snapshot/main/commands/generate-snapshot.md
curl -o ~/.claude/commands/update-snapshot.md https://raw.githubusercontent.com/YOUR_USERNAME/claude-snapshot/main/commands/update-snapshot.md
```

## Usage

### First time (in any repo)

```
/generate-snapshot
```

Claude will:
1. Explore your entire codebase using parallel agents
2. Write a comprehensive `codebase-snapshot.md` to persistent memory
3. Write a `MEMORY.md` that instructs future sessions to use the snapshot
4. Report what was captured and the git commit it's pinned to

### After making changes

```
/update-snapshot
```

Claude will:
1. Load the existing snapshot
2. Run `git diff` to find what changed since the snapshot commit
3. Read only the changed files
4. Patch the snapshot with updates
5. Bump the snapshot commit hash

### Future sessions (automatic)

You don't need to do anything. Claude automatically reads `MEMORY.md` at session start, which tells it to load the snapshot and check for diffs. No more Explore agents needed.

## What Gets Captured

The snapshot includes:
- **Directory tree** with file descriptions and line counts
- **API endpoints** (method + path + description)
- **Data models** (schemas, types, interfaces with fields and relations)
- **State management** (stores/contexts with state fields and actions)
- **Architecture patterns** (error handling, auth, validation, etc.)
- **Configuration** (build tools, env vars, deployment)
- **Dependencies** (key packages and their purpose)

## When to Regenerate

Run `/update-snapshot` when:
- You've made changes and want the snapshot current
- A few files changed since the last snapshot

Run `/generate-snapshot` again when:
- Major refactors changed the project structure
- The diff from the snapshot has grown very large (>15 files)
- You want a clean baseline

## How It Works

Claude Code has two features that make this possible:

1. **Persistent memory directory** (`~/.claude/projects/<path>/memory/`) — Files here survive across conversations
2. **Auto-loaded MEMORY.md** — This file is injected into every new conversation's context automatically

The generate command writes a comprehensive snapshot to the memory directory and sets up MEMORY.md to point future sessions at it. The update command incrementally patches the snapshot using git diff.

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI
- A git repository (uses git commit hashes and diffs for incremental updates)

## License

MIT
