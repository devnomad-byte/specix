---
name: spark
description: Use for exploration, ideation, or before building — Socratic inquiry through clarify, challenge, compare, and confirm. Light mode for quick exploration, deep mode for new features.
---

# Spark: Socratic Ideation

**Module type:** Clay (flexible)
**Trigger:** Before any new feature, large refactor, ambiguous requirement, or when the user wants to explore ideas.
**Alias prefix:** `specix:spark`

---

## Purpose

Spark prevents the costliest mistake in software: building the wrong thing fast. It is both a thinking partner for exploration and a structured gate before committing to a direction.

<HARD-GATE>
Do NOT invoke any implementation skill, write any code, or take any implementation action until the design direction has been confirmed by the user. This applies to EVERY request regardless of perceived simplicity.
</HARD-GATE>

## Anti-Pattern: "This Is Too Simple To Need Spark"

Every change benefits from examination. "Simple" changes are where unexamined assumptions cause the most wasted work. The process can be short for trivial items, but you MUST challenge assumptions and get confirmation before proceeding.

---

## Two Modes

### Light Mode — Quick Exploration

When the user wants to discuss, compare, or investigate without committing:

```
1. LISTEN — restate their question to confirm understanding
2. PROBE — ask ONE focused question at a time
3. INVESTIGATE — search codebase, map dependencies, trace data flows
4. COMPARE — present options with tradeoffs when multiple paths emerge
5. ILLUSTRATE — use ASCII diagrams when they clarify structure
```

Light mode produces **zero files**. All output lives in conversation.

### Deep Mode — Structured Ideation

When the user has a concrete change request or is about to start building:

```
1. GATHER — collect context (specs, codebase, constraints)
2. CLARIFY — one question at a time to refine understanding
3. CHALLENGE — question assumptions, reframe, surface risks
4. COMPARE — present 2-3 approaches with tradeoffs
5. CONFIRM — lock scope in/out, assumptions, risks
6. SELF-AUDIT — check for contradictions, ambiguity, scope creep
7. HAND OFF — invoke specix:draft
```

Deep mode also produces **zero files**. The only output is a confirmed direction.

Gateway routes to light mode when it detects exploratory intent ("I'm thinking about...", "What if we..."). Gateway routes to deep mode for concrete change requests or before Full/Lean paths.

---

## Light Mode Details

### Probe

Ask focused, sequential questions:
- "What's the main pain point you're trying to solve?"
- "Who are the primary users of this feature?"
- "What would break if we changed X?"

Never list five questions. One. Wait for the answer.

### Investigate

- Search the codebase for relevant patterns
- Map dependencies with ASCII diagrams
- Trace data flows or call chains
- Identify constraints and assumptions

### Compare

| Option | Pros | Cons | Effort |
|--------|------|------|--------|
| A      | ...  | ...  | Low    |
| B      | ...  | ...  | Medium |

State a recommendation, but leave the choice to the user.

### Transition Signals

| Signal | Next Step |
|--------|-----------|
| "Let's go with option B" | Enter deep mode → specix:draft |
| "How would we implement this?" | Still light — answer, don't build |
| "Start working on this" | specix:draft to formalize |
| "I just need to fix..." | specix:probe for bug investigation |

---

## Deep Mode Details

### Step 1 — Gather

- Scan `.specix/` for existing charters, deltas, blueprints
- Read `project.yaml` for tech stack, constraints, conventions
- Ask the user to describe the outcome, not the solution
- Inventory affected files, modules, and boundaries

### Step 2 — Clarify

One question per message. Multiple choice preferred, open-ended when genuinely unclear.

Good: "When this feature is done, what does the user see that they cannot see today?"
Bad: "Tell me everything about what you want."

### Step 3 — Challenge

**This step separates thinking from typing.**

Challenge the user's assumptions:
- "You said you need X — is X the problem or a symptom of Y?"
- "You want caching — have you measured current latency?"

Challenge your own assumptions:
- "What am I assuming that might be wrong?"

Challenge necessity (YAGNI):
- "Does this need to exist? Can existing X already solve this?"
- "What would happen if we did nothing?"

Reframe the problem:
- "What if the real issue isn't speed but reliability?"

Surface risks:
- "What could go wrong if we build this?"
- "Who else is affected that we haven't considered?"

### Step 4 — Compare

| Column | Meaning |
|--------|---------|
| Approach | One-line summary |
| Time | small / medium / large |
| Risk | What could go wrong |
| Benefit | What this uniquely enables |
| Cost | What this sacrifices |

Never present only one option.

### Step 5 — Confirm

Lock in writing:
- What will be built (scope in)
- What will NOT be built (scope out)
- Assumptions that, if wrong, would change the plan
- Risks acknowledged and accepted

### Step 6 — Self-Audit

| Check | Question |
|-------|----------|
| Placeholders | Any "TBD", "TODO", or vague language? |
| Contradictions | User said X early, Y later — which is current? |
| Scope | Small enough for one change, or needs splitting? |
| Ambiguity | Any requirement interpretable two ways? |
| YAGNI | Anything included that wasn't asked for? |

### Step 7 — Hand Off

> "Direction confirmed. Proceeding to specix:draft for formal documentation."

---

## Stance

| Be | Don't Be |
|----|----------|
| Curious, not prescriptive | Don't follow a rigid script |
| Open — surface multiple threads | Don't funnel through a single path |
| Challenging — question everything | Don't accept claims without evidence |
| Patient — let the problem emerge | Don't rush to solutions |
| Grounded — explore the actual codebase | Don't just theorize |
| Honest — say what you don't know | Don't fake understanding |

---

## Boundaries

Spark stays in conversation. It does not:
- Create `.specix/` directories or files
- Modify any source code
- Run tests or build commands
- Dispatch sub-agents

---

## Exit Conditions

| Condition | Action |
|-----------|--------|
| User confirms direction (deep mode) | Hand off to specix:draft |
| User satisfied with answers (light mode) | End spark, no artifacts |
| User wants to explore more | Continue questioning |
| User asks to implement immediately | Refuse. Explain hard gate. |
| Request too large for one change | Decompose into sub-changes |

---

## Anti-Patterns

- **Solution attachment:** Only asking questions that validate the first idea
- **Feature creep:** Letting scope expand during questions
- **Skipping tradeoffs:** Presenting one option as universally superior
- **Confirmation bias:** Only supporting the user's initial premise
- **Rushing past "why":** Jumping to "how" before confirming the problem is real
