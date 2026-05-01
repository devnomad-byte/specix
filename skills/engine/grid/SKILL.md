---
name: grid
description: Use when executing implementation plans — dispatches isolated worker agents per task with review and parallel scheduling
---

# Grid: Multi-Agent Dispatch

**Module type:** Iron (rigid execution pipeline)
**Trigger:** When implementation steps are ready for execution.
**Alias prefix:** `specix:grid`

---

## Purpose

Grid is the execution engine. It takes decomposed implementation steps (from specix:build) and runs each through a controlled pipeline: dispatch a worker, review the output, advance.

---

## Per-Task Cycle

```
TASK RECEIVED
    |
    v
STAGE 1: DISPATCH WORKER
    |  Worker receives full task context (isolated, no pollution)
    |  Worker implements, runs tests, reports completion
    v
STAGE 2: REVIEW
    |  Reviewer reads actual code files (not worker's report)
    |  Checks spec compliance (PASS/GAP/DRIFT/EXTRA)
    |  Checks critical code issues (errors, types, logic bugs)
    |  FAIL -> return to worker with specific issues
    |  PASS -> task complete
    v
TASK COMPLETE
```

See `review.md` for the reviewer template.

### Worker Isolation

Every worker starts with a **fresh context window**. Workers receive:
- The full task description with code and file paths
- The relevant spec sections (deltas) for their task
- Verification commands to run
- Self-review checklist (see `worker.md`)

Workers do NOT receive:
- Context from previous tasks
- Other workers' code (unless their task depends on it)
- Review feedback from other tasks

---

## Model Selection

| Task Characteristic | Model Tier | Examples |
|---------------------|-----------|----------|
| Mechanical, template-driven | Fast | CRUD endpoints, boilerplate |
| Standard logic | Standard | Business logic, data transforms |
| Architecture decisions, review | Strongest | Spec compliance review, integration points |

Workers → fast/standard. Reviewer → strongest.

---

## Parallel Scheduling

Grid builds a dependency graph from cluster grouping:

```
Cluster A (no deps) ─── Step 1, Step 2     <- parallel
Cluster B (deps on A) ── Step 3, Step 4     <- after A
Cluster C (deps on B) ── Step 5             <- after B
```

**Rules:**
- Independent tasks run in parallel (max 3 concurrent)
- Dependent tasks run in topological order
- If a task in a parallel batch fails, siblings continue

---

## Failure Handling

| Failure | Response |
|---------|----------|
| Worker fails to implement | Retry once with expanded context. Still fails → escalate to user. |
| Review finds GAP/DRIFT | Return to worker with specific items. Max 2 retries. |
| Review finds critical code issue | Return to worker with fix list. Max 2 retries. |
| Task fails after max retries | Pause grid, report to user. |

---

## Grid Completion

When all tasks pass:

1. Run specix:proof on the entire changeset
2. If proof passes → hand off to specix:audit
3. If proof fails → fix and re-run proof

---

## Output

```
GRID COMPLETE

Tasks: 8/8 passed
Review notes for specix:lens: 2 notes
Worker retries: 1

Proceeding to specix:proof → specix:audit.
```
