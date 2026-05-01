---
name: build
description: Use when specification artifacts are complete and you are ready to implement — validates lineup is executable, then dispatches workers via specix:grid
---

# specix:build — Implementation Engine

You coordinate implementation. You validate the lineup is directly executable, then dispatch workers through specix:grid.

**Module type:** Flow (sequential with parallel branches)
**Alias prefix:** `specix:build`

---

## When to Activate

- All artifacts exist in a change directory (charter, deltas, blueprint, lineup)
- User says "implement this" or "start building"
- Gateway routes to Full or Lean path after specix:draft completes
- User types `/specix:build`

## Prerequisites Check

Verify the active change has:

```
.specix/changes/<name>/
  ├── charter.md      # Must exist
  ├── deltas/*.md     # At least one delta file
  ├── blueprint.md    # Must exist
  └── lineup.md       # Must exist with executable tasks
```

If any artifact is missing, stop and direct the user to `specix:draft`.

## Change Selection

| Situation | Action |
|-----------|--------|
| `.specix/changes/` has one subfolder | Auto-select it |
| User specifies a name | Use that name |
| Multiple changes, no name given | List all with progress, ask user to pick |

---

## Step 1 — Validate Lineup

Read `lineup.md`. Verify every task is directly executable:

### Validation Checklist

| Check | What to Look For |
|-------|-----------------|
| File paths | Every task lists exact files to create or modify |
| Step detail | Every checkbox step describes a concrete action |
| Code blocks | Steps that need code include the actual code (not pseudocode) |
| Verify commands | Every task has at least one runnable verification command |
| No placeholders | No "TBD", "TODO", "fill in later", "similar to X", "as needed" |
| Dependencies | Tasks are ordered so dependencies come first |

### Validation Outcomes

| Result | Action |
|--------|--------|
| All checks pass | Proceed to Step 2 |
| Missing file paths or commands | Fill in the gaps directly in lineup.md |
| Contains placeholders or vague steps | Decompose those tasks into concrete steps, update lineup.md |
| Task is too large (more than 5 steps) | Split into smaller tasks, update lineup.md |

Any fixes to lineup.md should preserve the task numbering and group structure.

### Dependency Clustering

After validation, group tasks into clusters for specix:grid:

```
## Cluster A: Foundation (no dependencies)
  Task 1.1: Create database schema
  Task 1.2: Define TypeScript types

## Cluster B: Core logic (depends on A)
  Task 2.1: Implement repository layer
  Task 2.2: Build service functions

## Cluster C: Integration (depends on B)
  Task 3.1: Wire API endpoints
```

---

## Step 2 — Execute (Grid)

Dispatch the validated plan through specix:grid:

```
CALL specix:grid
  - Independent tasks (same cluster) run in parallel (max 3)
  - Dependent tasks run in topological order
  - Each task: implement → review → advance
  - On failure: call specix:probe for diagnosis
  - During implementation: follow specix:redgreen discipline
```

Workers receive the task content directly from lineup.md — including file paths, code, and verify commands. No re-interpretation needed.

---

## Step 3 — Track Progress

After each task completes:

1. Open `lineup.md`
2. Replace `- [ ] **Step K:**` with `- [x] **Step K:**`
3. When all steps in a task are done, the task is complete
4. If partially failed, mark the failed step with a note

This makes progress visible across sessions. If the session ends mid-build, the next session reads `lineup.md` and resumes exactly where it left off.

---

## Failure Handling

| Failure Type | Response |
|-------------|----------|
| Single task compilation error | Call `specix:probe` to diagnose, fix, retry |
| Task produces wrong output | Re-read deltas/blueprint, adjust implementation |
| Multiple tasks failing | Pause. Report pattern to user. |
| External dependency missing | Skip blocked tasks, continue unblocked ones. |
| Lineup task too vague after validation | Decompose further, update lineup.md, continue |

---

## After Completion

When all lineup items are checked:

1. Present a build summary: tasks completed, files changed, tests status
2. State: "Build phase complete. Next: `specix:proof` to verify everything runs."
3. Do NOT proceed to audit without user confirmation

The handoff is explicit. Build stops here. `specix:proof` and `specix:audit` are separate decisions.
