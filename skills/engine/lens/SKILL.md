---
name: lens
description: Use after audit passes — dispatches reviewer agent for holistic code review with optional specialist lenses for security, performance, and architecture
---

# Lens: Code Review with Specialists

**Module type:** Flow (sequential)
**Trigger:** After specix:audit passes and before specix:branch.
**Alias prefix:** `specix:lens`

---

## Purpose

Lens provides holistic code review that looks beyond individual tasks. While specix:grid reviewed each task in isolation, lens reviews the entire changeset as a cohesive whole — checking for cross-task consistency, architectural soundness, and systemic quality.

---

## Review Structure

### Stage 1: General Review

Dispatch the reviewer persona (see `reviewer.md`) for an overall assessment covering:

- Plan alignment — does the code deliver what the charter promised?
- Code quality — consistent style, clean abstractions, readable logic
- Architecture — reasonable patterns, appropriate coupling
- Issue inventory — categorized as Critical / Important / Suggestion

### Stage 2: Specialist Lenses (Conditional)

Based on the nature of the changes, activate specialist reviewers from the `specialists/` directory:

| Specialist | When to Activate |
|-----------|-----------------|
| Security | Any code handling authentication, authorization, user input, file operations, network requests, or financial data |
| Performance | Any code involving database queries, loops over large datasets, caching, API response times, or memory-intensive operations |
| Architecture | Changes affecting module boundaries, shared interfaces, dependency direction, or cross-cutting concerns |

Activation is not optional when the trigger condition is met. If the change touches authentication, the security specialist MUST review.

---

## Receiving Feedback Protocol

When the reviewer returns feedback, follow this protocol exactly:

### 1. READ

Read every comment. Read the full context around each comment. Do not skim.

### 2. UNDERSTAND

Reproduce the reviewer's reasoning. Why did they flag this? What chain of thought led from the code to the concern?

### 3. VERIFY

Check the reviewer's claim against the actual code. Reviewers can be wrong. If a comment says "this function has no error handling," verify that claim by reading the function.

### 4. EVALUATE

Classify each comment:

| Category | Action |
|----------|--------|
| Correct and actionable | Implement the suggested fix or a better alternative |
| Correct but suggestion is suboptimal | Implement the fix with a better approach, explain the improvement |
| Incorrect claim | Provide evidence-based pushback with code citations |
| Opinion/preference | Acknowledge and consider; implement if it improves clarity |

### 5. RESPOND

For each comment, provide a technical response:
- If implementing: "Fixed in [file:line]. [Brief description of the change.]"
- If pushing back: "This comment appears incorrect because [specific evidence]. The code at [file:line] already handles this case via [mechanism]."
- If deferring: "Noted as Suggestion. Not addressing in this changeset because [reason]."

### 6. IMPLEMENT

Apply all accepted changes. Run specix:proof after implementation.

---

## Prohibited Responses

The following responses are strictly forbidden because they represent social compliance rather than technical evaluation:

- "Good point!" (no technical substance)
- "Thanks for catching that!" (social, not technical)
- "Will fix!" (no analysis of whether the fix is correct)
- "Agreed." (no independent verification)
- Silent implementation without understanding (copying suggestions blindly)

Every response must demonstrate that you independently verified the claim and evaluated its merit.

---

## Specialist Integration

Specialist reviews run independently and produce their own reports. The receiving protocol applies equally to specialist feedback — verify their claims independently.

Specialist reports are appended to the general review summary:

```
LENS REVIEW COMPLETE

General review: [PASS | CONDITIONAL | FAIL]
Security specialist: [PASS | CONDITIONAL | FAIL] (if activated)
Performance specialist: [PASS | CONDITIONAL | FAIL] (if activated)
Architecture specialist: [PASS | CONDITIONAL | FAIL] (if activated)

Combined issue count: [C] Critical, [I] Important, [S] Suggestion
```

---

## Completion

- All Critical issues resolved -> PASS -> proceed to specix:branch
- Important issues remain -> CONDITIONAL PASS -> may proceed with documented exceptions
- Critical issues unresolved -> FAIL -> fix and re-run lens
