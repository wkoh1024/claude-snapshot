# Update Codebase Snapshot

You are updating an existing codebase snapshot to reflect recent changes.

## Instructions

### Step 1: Load current snapshot

Read `codebase-snapshot.md` from your persistent memory directory. Find the **Snapshot Commit** hash at the top.

If no snapshot exists, tell the user to run `/generate-snapshot` first.

### Step 2: Find what changed

Run these commands:
```
git log --oneline [SNAPSHOT_COMMIT]..HEAD
git diff --stat [SNAPSHOT_COMMIT]..HEAD
```

If nothing changed, tell the user the snapshot is up to date and stop.

### Step 3: Read changed files

For each file that changed (from git diff --stat), read it to understand what's new or different. Focus on:
- New files (need full descriptions added to snapshot)
- Deleted files (remove from snapshot)
- Modified files (update descriptions, endpoints, types, etc.)
- New dependencies in package.json or equivalent
- Schema/migration changes

### Step 4: Update the snapshot

Edit `codebase-snapshot.md` to reflect the changes:
- Update the **Snapshot Commit** hash and date at the top
- Add/remove/update file entries in the directory tree
- Add/remove/update API endpoints
- Add/remove/update data models
- Update any patterns or architecture notes that changed

Do NOT rewrite the entire snapshot — only modify the sections affected by the changes.

### Step 5: Update MEMORY.md

Update the snapshot commit hash in `MEMORY.md`.

### Step 6: Report results

Tell the user:
- How many commits were incorporated
- What sections of the snapshot were updated
- The new snapshot commit hash

$ARGUMENTS
