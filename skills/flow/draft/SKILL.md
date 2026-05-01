---
name: draft
description: Use when you have a confirmed change idea and need to produce formal specification artifacts — charter, deltas, blueprint, and lineup
---

# specix:draft — Specification Author

You turn confirmed ideas into structured artifacts. Your output lives in `.specix/changes/<change-name>/`.

## When to Activate

- User has a clear change request and wants formal planning
- Spark concluded with a direction worth documenting
- Gateway routes to Full or Lean path
- User types `/specix:draft <description>`

## Five-Step Process

```
Step 1: INIT (if needed)
  |
  v
Step 2: CHARTER
  |
  v
Step 3: DELTAS  ──── concurrent ────> Step 4: BLUEPRINT
  |                                      |
  v                                      v
Step 5: LINEUP (depends on both deltas + blueprint)
```

### Step 1 — Init Project

Check if `.specix/` exists at project root.

**If missing, create the full structure:**

```
.specix/
  ├── project.yaml       # Project identity and rules
  ├── specs/             # Master specification documents
  ├── changes/           # Active changes in progress
  ├── archive/           # Completed and merged changes
  └── learn/             # Session insights (JSONL)
```

**project.yaml template:**

```yaml
schema: spec-driven

context: |
  # TODO: Fill in — tech stack, frameworks, database, deployment target

rules:
  charter: []    # Rules for charter artifact generation
  deltas: []     # Rules for delta spec generation
  blueprint: []  # Rules for design document generation
  lineup: []     # Rules for task breakdown generation
```

If `.specix/` already exists, skip init. Read `project.yaml` for context.

**Ask the user for a change name** (kebab-case, e.g., `user-auth`, `payment-v2`). Create `.specix/changes/<change-name>/`.

### Step 2 — Charter

Produce `charter.md` — the "why and what" document.

Structure:
- **Motivation**: Why this change exists. What problem it solves. For whom.
- **Scope**: What changes and what stays the same. Boundaries.
- **Impact**: Who is affected. What breaks. Migration concerns.
- **Success criteria**: How we know this change is done and correct.

Inject `project.yaml > rules > charter` constraints.

### Step 3 — Deltas

Produce `deltas/*.md` — incremental specification changes.

Each delta file covers one module or domain area. Within each file, use labeled blocks:

```
#### NEW: RequirementName
Description of the new requirement. Use SHALL for mandatory, SHOULD for recommended.

#### EDIT: ExistingRequirementName
What changes about this existing requirement.

#### DROP: ObsoleteRequirementName
Why this requirement is being removed.

#### RENAME: OldName → NewName
The requirement is being renamed; subsequent operations reference NewName.
```

Inject `project.yaml > rules > deltas` constraints.

### Step 4 — Blueprint

Produce `blueprint.md` — the technical design document.

Structure:
- **Architecture decisions**: Key technical choices and rationale
- **Data model changes**: Schema additions, modifications, removals
- **Interface contracts**: API endpoints, events, function signatures
- **Dependency map**: What this change touches and depends on

Inject `project.yaml > rules > blueprint` constraints.

### Step 5 — Lineup

Produce `lineup.md` — an ordered task checklist.

Each task:
- Starts with `- [ ]` (unchecked)
- Has a short title and a one-line description
- References which delta or blueprint section it fulfills
- Is estimated at 2-5 minutes of focused work

Inject `project.yaml > rules > lineup` constraints.

## Artifact Dependencies

```
charter.md (no dependencies)
     |
     +---> deltas/*.md (requires charter for motivation/scope)
     +---> blueprint.md (requires charter for scope)
              |
              v
         lineup.md (requires both deltas and blueprint)
```

Generate in topological order. Never produce lineup before deltas and blueprint are complete.

## Context Injection

Before generating any artifact, read `project.yaml` and incorporate:

- **context**: Adds project-specific background (tech stack, conventions, constraints)
- **rules[artifact-type]**: Applies mandatory formatting, content, or quality rules

If `project.yaml` has unfilled TODO placeholders, proceed normally but note to the user that filling them in will improve future artifact quality.

## After Completion

Present a summary of all generated artifacts. Then suggest:

> "Artifacts are ready. Next step: `/specix:build` to begin implementation."

If the user wants to revise, iterate on the specific artifact rather than regenerating everything.
