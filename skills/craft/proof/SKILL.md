---
name: proof
description: Use before claiming any work is complete — iron gate that requires fresh verification evidence for every assertion
---

# Proof: Evidence-Based Completion

**Module type:** Iron (rigid — cannot be skipped or bypassed)
**Trigger:** Automatically, before any completion claim.
**Alias prefix:** `specix:proof`

---

## Iron Law

> **"No completion claim without fresh verification evidence."**

Saying "it works" without running commands and reading their output is a lie. This module exists to prevent that lie.

---

## The 5-Step Gate Function

Every completion claim must pass through this gate. The steps are sequential and non-negotiable.

### 1. IDENTIFY

State exactly what was done and what "done" means for this work:

```
Work completed:
- [specific change 1]
- [specific change 2]

Definition of done:
- [test suite X passes]
- [command Y exits with code 0]
- [file Z contains expected content]
```

Vague claims like "fixed the bug" or "implemented the feature" are rejected. Be specific about what changed and what success looks like.

### 2. RUN

Execute the verification commands. Not last hour's commands. Not yesterday's commands. Right now.

```bash
# Run the full test suite
npm test

# Run the type checker
npx tsc --noEmit

# Run the linter
npm run lint
```

Every command must be executed in the current session. Cached results are not evidence.

### 3. READ

Read the full output of every command. Not a summary. Not "it passed." The actual output.

Look for:
- Exit codes (0 is not the only thing that matters)
- Warnings that were not there before
- Deprecation notices
- Test counts (did the number of tests change unexpectedly?)
- Coverage numbers (did coverage drop?)

### 4. VERIFY

Confirm each criterion from the IDENTIFY step against the actual output:

| Criterion | Evidence | Status |
|-----------|----------|--------|
| Test suite passes | Output shows "47 passed, 0 failed" | CONFIRMED |
| No type errors | `tsc` exits 0, no error lines | CONFIRMED |
| New test covers the fix | Test file `X` includes case `Y` | CONFIRMED |

If any criterion cannot be confirmed, the work is NOT complete.

### 5. CLAIM

Only after steps 1-4 pass, make the completion claim:

```
VERIFICATION COMPLETE

Evidence:
- npm test: 47 passed, 0 failed (exit 0)
- tsc --noEmit: no errors (exit 0)
- npm run lint: 0 problems (exit 0)

All criteria from IDENTIFY step confirmed.
Work is complete.
```

---

## What Counts as Evidence

| Evidence Type | Acceptable? | Notes |
|---------------|------------|-------|
| Command output from THIS session | YES | The gold standard |
| Test results from THIS session | YES | With full output |
| File contents verified by reading | YES | For structural changes |
| "I ran it earlier and it worked" | NO | Stale evidence |
| "The code looks correct" | NO | Inspection is not execution |
| "Should be fine" | NO | Hope is not evidence |
| "Tests should still pass" | NO | Run them |
| Previous session's output | NO | Not fresh |
| CI pipeline passed | CAUTION | Acceptable only if CI ran on the exact current commit |
| Linter reported no issues yesterday | NO | Run it again |

---

## Automatic Trigger

Proof is triggered automatically in these situations — the developer does not choose when to use it:

- Before any commit message that claims a fix or feature
- Before declaring implementation of a task complete
- Before moving from one task to the next in specix:grid
- Before invoking specix:audit (verify module)
- Before invoking specix:lens (review module)
- Before invoking specix:branch (branch completion)

Skipping proof at any of these checkpoints is a pipeline violation.

---

## Failure Protocol

When proof fails:

1. Do NOT claim completion
2. Identify which criterion failed
3. Return to the failing work and fix it
4. Re-run the full proof gate from step 1

Do not patch and re-verify only the failing criterion. Run the entire gate again because fixes can introduce new failures.

---

## Interaction with Other Modules

Proof is a checkpoint, not a destination. After passing:

| Context | Next Module |
|---------|-------------|
| Task completed in specix:grid | Continue to next task or conclude grid session |
| All tasks done | specix:audit (spec verification) |
| Bug fix applied | specix:audit, then specix:lens if fix is large |
| Hotfix for production | Proof + immediate deploy |

---

## The One Exception

There is exactly one situation where proof may be relaxed: **direct trivial edits** that do not affect runtime behavior (typos in comments, documentation-only changes). In this case, a file-read verification is sufficient instead of command execution.
