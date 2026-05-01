---
name: isolate
description: Use when starting feature work that needs isolation from the current workspace — creates independent git worktrees with safety verification
---

# Isolate: Workspace Isolation

**Module type:** Flow (sequential)
**Trigger:** When starting work that should be isolated from the current branch.
**Alias prefix:** `specix:isolate`
**Prerequisite:** Git must be available. If the project has no git repository, this module is unavailable — skip directly to `specix:build`.

---

## Purpose

Isolate creates a clean git worktree — an independent working directory that shares the same repository but operates on its own branch. This prevents unfinished work from contaminating the main development line and allows parallel workstreams without stashing or committing half-done changes.

---

## Directory Selection Priority

When creating a worktree, choose the location using this priority order:

1. `.specix/worktrees/<branch-name>/` inside the project root — keeps worktrees co-located with the project
2. System temp directory with a project-prefixed path — fallback when project directory is not writable
3. User-specified path — when the user explicitly provides a location

Never create a worktree inside `node_modules/`, `src/`, or any directory that contains tracked project code.

---

## Creation Process

### Step 1: Pre-Flight Checks

Before creating anything, verify:

- [ ] Current working directory is a git repository (`git rev-parse --git-dir` succeeds)
- [ ] No uncommitted changes in the current branch (or the user has explicitly agreed to stash them)
- [ ] The target branch name does not already exist
- [ ] There is enough disk space for a second working tree

If any check fails, report the issue and wait for resolution.

### Step 2: Create the Worktree

```bash
# Create a new branch and worktree in one step
git worktree add <worktree-path> -b <branch-name> <base-branch>
```

- `<base-branch>` is determined by reading `.specix/project.yaml` if it specifies a base branch, otherwise defaults to the current branch
- `<branch-name>` is derived from the change name (kebab-case, e.g., `feat/user-auth`)

### Step 3: Verify the Worktree

After creation, verify:

- [ ] `git -C <worktree-path> status` shows a clean tree
- [ ] `git -C <worktree-path> log -1` matches the base branch's HEAD
- [ ] `git -C <worktree-path> branch --show-current` shows the new branch name
- [ ] The worktree directory is accessible and writable

### Step 4: Configure Environment

Within the new worktree:

- [ ] Install dependencies (`npm install` or equivalent for the project)
- [ ] Verify the test suite runs (`npm test` or equivalent)
- [ ] Copy `.specix/` configuration if it is not already present (worktrees share the same `.git` so local config may need adjustment)

---

## .gitignore Verification

After creating the worktree, confirm that `.gitignore` in the new worktree covers:

- `node_modules/` or equivalent dependency directories
- Build output directories (`dist/`, `build/`, `.next/`, etc.)
- Environment files (`.env`, `.env.local`)
- Editor/IDE configuration (`.idea/`, `.vscode/` — unless project-shared)
- OS-generated files (`.DS_Store`, `Thumbs.db`)

If any of these are missing from `.gitignore`, add them. Do not proceed with development in a worktree that might accidentally commit generated or secret files.

---

## Safety Checks

These checks run during worktree creation and before any destructive operation:

| Check | Failure Response |
|-------|-----------------|
| Uncommitted changes in current branch | Warn user, offer to stash or abort |
| Branch name already exists | Suggest alternative name or offer to check out existing branch |
| Worktree path already exists | Report conflict, suggest alternative path |
| Test suite fails in new worktree | Report failure, do not use the worktree until resolved |
| Disk space below threshold | Warn user, suggest cleanup |

---

## Working in the Worktree

Once the worktree is active:

- All implementation work happens in the worktree directory
- Spec files (`.specix/changes/<name>/`) are created relative to the worktree root
- specix:grid, specix:redgreen, specix:proof, and other modules operate normally within the worktree

---

## Cleanup

When the worktree is no longer needed (after specix:branch completes):

```bash
git worktree remove <worktree-path>
```

Verify removal with `git worktree list`. No stale worktrees should remain.

---

## Output

```
ISOLATE COMPLETE

Worktree path: <absolute-path>
Branch: <branch-name>
Base: <base-branch>
Test baseline: [PASS | FAIL — do not use this worktree]

Ready for development. All specix: modules will operate in:
  <worktree-path>
```
