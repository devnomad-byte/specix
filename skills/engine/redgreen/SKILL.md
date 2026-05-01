---
name: redgreen
description: Use when implementing any feature or fix — enforces the RED-GREEN-REFACTOR cycle: failing test first, minimal implementation, then cleanup
---

# Redgreen: Test-Driven Development

**Module type:** Iron (rigid — the cycle must be followed exactly)
**Trigger:** Before writing any production code for a feature or fix.
**Alias prefix:** `specix:redgreen`

---

## Iron Law

> **"No production code without a failing test first."**

If you write implementation code before a test exists that demands it, you are not doing TDD. You are writing code and hoping tests will confirm it later. The test must come first because the test is the specification of the next behavior you are about to add.

---

## The RED-GREEN-REFACTOR Cycle

This cycle repeats for every discrete behavior. A "behavior" is the smallest testable unit of functionality.

### RED — Write a Failing Test

1. Write a test that describes the behavior you want to add
2. Run the test — it MUST fail (and fail for the right reason, not a syntax error)
3. The test failure message should clearly state what is missing

**RED exit criteria:**
- Test fails with a message that names the missing behavior
- The failure is not a compilation error or import error (those are setup problems, not test failures)
- You understand exactly what code would make this test pass

### GREEN — Write the Minimum Implementation

1. Write the simplest code that makes the failing test pass
2. Do not add features the test does not require
3. Do not optimize, generalize, or abstract — just pass the test
4. Run the test — it MUST pass

**GREEN exit criteria:**
- The previously failing test now passes
- No other tests broke (run the full suite)
- The implementation is minimal — no extra logic beyond what the test demands

### REFACTOR — Clean Up Without Changing Behavior

1. With all tests passing, now improve the code
2. Rename variables, extract functions, remove duplication
3. After each refactor step, run tests — they must still pass
4. If a refactor breaks a test, revert immediately and try a different approach

**REFACTOR exit criteria:**
- All tests pass (same as before refactor)
- Code is cleaner (better names, less duplication, clearer structure)
- No new behavior was added

---

## The Cycle in Practice

```
Write test for behavior A -> RED (fails)
Implement behavior A -> GREEN (passes)
Clean up -> REFACTOR (still passes)
    |
Write test for behavior B -> RED (fails)
Implement behavior B -> GREEN (passes)
Clean up -> REFACTOR (still passes)
    |
... repeat until all behaviors are implemented
```

Each cycle should take 2-5 minutes. If a cycle takes longer, the behavior is too large — split it.

---

## Red Flags: Excuses vs. Reality

| What You Hear | What It Actually Means | Required Response |
|---------------|----------------------|-------------------|
| "I'll write tests after" | You are avoiding the discipline that catches design mistakes early | Write the test first. Now. |
| "This is too simple to test" | Every bug was once "too simple to test" | If it can be wrong, it can be tested. |
| "I need to write the implementation to understand the problem" | You have not thought about the problem enough | Think more, then write the test that describes the solution. |
| "TDD slows me down" | You are measuring typing speed, not delivered quality | Measure hours spent debugging vs. hours spent writing tests. |
| "The architecture isn't settled yet" | Perfect reason to write tests that specify the interface you want | Write tests against the desired interface, let implementation follow. |
| "This is a bug fix, not a feature" | The bug exists because there was no test covering this case | Write a test that reproduces the bug first. |
| "I can't test this without mocking everything" | The design has tight coupling to infrastructure | The test is telling you to redesign for testability. |

---

## When to Stop Cycling

The cycle ends when:

1. Every behavior from the spec has a corresponding test
2. All tests pass (GREEN)
3. Code has been refactored for clarity
4. specix:proof confirms the evidence

Then invoke specix:proof for the final verification gate.

---

## Reference

For detailed examples of what NOT to do, see `antipatterns.md` in this module directory.
