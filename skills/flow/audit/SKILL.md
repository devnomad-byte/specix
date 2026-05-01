---
name: audit
description: Use after implementation to verify code matches specifications AND solves the original problem — dual-layer audit with rollback logic
---

# specix:audit — Dual-Layer Verification

You are the quality gate between implementation and archival. Two questions must be answered:

1. **Did we build what the specs say?** (compliance)
2. **Did we build what was actually needed?** (relevance)

Both must pass. Neither alone is sufficient.

## When to Activate

- After `specix:proof` confirms all tests pass and code compiles
- User says "verify this" or "audit the implementation"
- Gateway routes to audit phase in any path
- User types `/specix:audit`

**Never run before `specix:proof`.** Proof ensures the code runs. Audit checks if it runs the right thing.

## Layer 1 — Spec Compliance

Read all deltas and blueprint from the active change. For each requirement block, verify:

| Dimension | Check |
|-----------|-------|
| Completeness | Every NEW/EDIT requirement has corresponding implementation |
| Correctness | Implementation behavior matches the SHALL/MUST statements |
| Consistency | No contradictions between implementation and blueprint decisions |

**Method:** Walk through each delta file requirement-by-requirement. For each one, find the corresponding code. Mark status.

**Compliance report format:**

```
| Delta File | Requirement | Status | Notes |
|------------|------------|--------|-------|
| auth.md | NEW: SessionToken | PASS | Implemented in session.ts |
| auth.md | EDIT: LoginFlow | GAP | Missing rate limiting (line 42 of delta) |
| api.md | NEW: HealthEndpoint | PASS | Returns correct schema |
```

Status values: `PASS`, `GAP` (missing piece), `DRIFT` (implemented differently than specified), `EXTRA` (implemented but not in spec).

## Layer 2 — Real Need Traceback

Open `charter.md`. Re-read the Motivation and Success Criteria sections.

For each success criterion, ask:
- Does the implemented code actually achieve this?
- Is the original problem solved?
- Did we drift into solving a different problem than the one stated?

**Traceback format:**

```
Charter motivation: "Users cannot recover forgotten passwords"
  -> Success criterion: "Password reset email sent within 30 seconds"
  -> Evidence: reset-password.ts triggers email dispatch
  -> Verdict: ADDRESSED

Charter motivation: "Users cannot recover forgotten passwords"
  -> Success criterion: "Reset link expires after 15 minutes"
  -> Evidence: Token expiry set to 24 hours in config
  -> Verdict: MISMATCH — link lives too long
```

## Three-Level Rollback

When audit finds problems:

### Level 1 — Small Deviation

```
1-2 requirements missed, or a detail drifted.
Fix the specific code directly.
Re-run specix:proof.
Re-run specix:audit (this module).
```

### Level 2 — Significant Drift

```
Architecture went wrong direction, or core feature missing.
Return to specix:build — re-plan only the failing tasks.
Re-run specix:proof.
Re-run specix:audit.
```

### Level 3 — Spec Was Wrong

```
The charter itself misunderstood the problem.
Return to specix:draft — revise charter, deltas, blueprint, lineup.
Re-run specix:build from the revised artifacts.
Full pipeline from build onward.
```

### Cycle Limit

**Maximum 3 audit cycles.** If after 3 rounds of fix-and-re-audit the code still doesn't pass:

- Stop the cycle
- Present a clear report of what keeps failing
- Ask the user for manual intervention
- Do not attempt a 4th cycle autonomously

## Ordering Constraint

```
specix:proof  -->  specix:audit  -->  specix:lens
              YOU ARE HERE      ^
                                 |
              audit must pass before lens runs
```

`specix:lens` (full code review) only runs on code that has passed audit. Reviewing code that doesn't meet its own spec is wasted effort.

## After Completion

Present the full audit report. State one of:

> "Audit PASSED. All requirements met and original problem addressed. Next: `specix:lens` for code quality review."

Or:

> "Audit FAILED. [N] gaps found. Rolling back to [level]. [Specific items to fix]."
