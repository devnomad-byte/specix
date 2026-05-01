---
name: probe
description: Use when encountering bugs, test failures, or unexpected behavior — four-phase root cause investigation before any fix
---

# Probe: Systematic Debugging

**Module type:** Iron (rigid — must be followed exactly)
**Trigger:** Bug reports, test failures, unexpected runtime behavior, regression alerts.
**Alias prefix:** `specix:probe`

---

## Iron Law

> **"No fix proposal without root cause identification."**

Attempting to fix a symptom without understanding its cause is guessing. Guessing in production code is unacceptable.

---

## The Four Phases

Phases must be completed in order. Skipping a phase is a violation of the Iron Law.

### Phase 1: Root Cause

Investigate until you can state the cause in one sentence.

**Actions:**
- Read the error message literally — what exact line, what exact value, what exact condition failed?
- Reproduce the issue with the minimal set of inputs
- Trace the data flow from input to failure point
- Read the actual code on the failing line and every function in the call stack

**Completion criteria:** A single sentence of the form:
> "The bug occurs because [specific code path] produces [specific wrong value] when [specific condition] is met."

If you cannot write this sentence, you have not found the root cause. Keep investigating.

### Phase 2: Pattern

Determine whether this is an isolated incident or a systemic pattern.

**Actions:**
- Search the codebase for the same failure pattern in other locations
- Check if related tests exist and what they cover
- Identify if this is a type error, logic error, integration error, or design error
- Determine if the fix is local (one spot) or structural (requires architectural change)

**Questions to answer:**
- Has this type of bug happened before in this project?
- Are there other code paths that share the same vulnerable pattern?
- Does the existing test suite have a gap that allowed this to escape detection?

### Phase 3: Hypothesis

Formulate a specific, testable hypothesis for the fix.

**Actions:**
- State the fix in terms of what changes, not how
- Predict the side effects of the fix — what else might change?
- Identify the narrowest possible fix vs. a broader structural fix
- Choose the approach with the smallest blast radius

**Format:**
```
Hypothesis: Changing [X] to [Y] will resolve the bug because [reason].
Predicted side effects: [list or "none expected"].
Risk level: [low | medium | high].
```

### Phase 4: Repair

Only now is writing a fix permitted.

**Actions:**
- Write a failing test that reproduces the bug (use specix:redgreen)
- Implement the minimal fix
- Run the full test suite — not just the new test
- Verify the original bug is gone and no regressions appeared

**After repair:** Invoke `specix:redgreen` for the test-driven fix cycle.

---

## Red Flags: Excuses vs. Reality

| What You Hear | What It Actually Means | Required Response |
|---------------|----------------------|-------------------|
| "It's a timing issue" | You haven't found the race condition yet | Map the concurrent execution paths |
| "It works on my machine" | Environment difference is the root cause | Diff the environments systematically |
| "The test is flaky" | There is a non-deterministic bug | Identify the source of randomness |
| "This is a known issue" | Nobody has debugged it properly | Debug it now with the same rigor |
| "It's probably a cache problem" | You're guessing | Clear the cache, reproduce, then investigate |
| "Let me just try this quick fix" | You're skipping probe entirely | Return to Phase 1 |

---

## When to Escalate

- Phase 1 exceeds 30 minutes without a root cause sentence — ask the user for domain context
- Phase 3 produces only high-risk hypotheses — present options to the user before proceeding
- The fix requires changes to 5+ files — consider whether this is actually a structural issue that warrants a charter

---

## Interaction with Other Modules

| After probe completes... | Next module |
|--------------------------|-------------|
| Bug is isolated, fix is small | specix:redgreen (TDD fix) |
| Bug reveals a design flaw | specix:spark (ideation for redesign) |
| Bug is in production, needs hotfix | specix:redgreen, then specix:proof for verification |
| Multiple related bugs found | specix:spark to decide on systematic vs. point fixes |

---

## Output

Probe produces its findings in conversation. The structured output is:

1. Root cause sentence
2. Pattern analysis (isolated vs. systemic)
3. Hypothesis statement
4. Repair summary (after fix is applied)

No files are written by probe itself. The fix is applied through specix:redgreen.
