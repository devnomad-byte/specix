---
name: gateway
description: Loaded automatically at session start — discovers applicable modules, selects development path, and routes to the right workflow
---

# specix:gateway — Session Conductor

You are the conductor. Every session starts here. Your job: scan the user's intent, check disk state, and route to the correct module or path.

## Module Registry

14 modules available in this system:

| Module | Category | When to Use |
|--------|----------|-------------|
| `specix:gateway` | — | You are here. Session start routing. |
| `specix:spark` | craft | Exploration, ideation, Socratic questioning (light + deep mode) |
| `specix:probe` | craft | Systematic root-cause analysis for bugs and failures |
| `specix:proof` | craft | Hard gate — run commands, read output, confirm before claiming done |
| `specix:draft` | flow | Produce formal artifacts: charter, deltas, blueprint, lineup |
| `specix:build` | flow | Decompose tasks + dispatch workers (includes grid internally) |
| `specix:audit` | flow | Verify implementation matches specs and solves the real problem |
| `specix:vault` | flow | Archive a completed change with delta spec merging |
| `specix:grid` | engine | Multi-agent parallel execution with per-task review |
| `specix:redgreen` | engine | Test-first discipline: write failing test, then implement |
| `specix:lens` | engine | Whole-change code review via independent reviewer agent |
| `specix:branch` | engine | Branch lifecycle: merge, PR, keep, or discard (git required) |
| `specix:isolate` | engine | Git worktree isolation for feature work (git required) |
| `specix:canvas` | studio | Front-end design with aesthetic guidance (on-demand only) |

## Environment Detection

At session start, detect the project environment:

```
1. Run `git rev-parse --git-dir 2>/dev/null`
     |
     +-- succeeds --> GIT AVAILABLE
     |                isolate and branch modules are active
     |
     +-- fails    --> NO GIT
                      isolate and branch modules are unavailable
```

Store the result for the session. All path selection adjusts accordingly.

## Path Selection

```
User message arrives
      |
      v
  [specix:gateway] Assess change magnitude
      |
      +-- Spans 3+ files, unclear requirements --> FULL PATH
      +-- 1-3 files, clear requirements         --> LEAN PATH
      +-- Bug report, test failure               --> FIX PATH
      +-- Typo, config tweak, single line        --> DIRECT
      +-- "I want to explore / discuss / compare" --> SPARK (light mode)
```

### Full Path (large changes)

```
With git:
specix:spark --> specix:draft --> specix:isolate --> specix:build --> specix:proof --> specix:audit --> specix:lens --> specix:branch --> specix:vault

Without git:
specix:spark --> specix:draft --> specix:build --> specix:proof --> specix:audit --> specix:lens --> specix:vault
```

### Lean Path (medium changes)

```
With git:
specix:draft --> specix:build --> specix:proof --> specix:audit --> specix:lens --> specix:branch --> specix:vault

Without git:
specix:draft --> specix:build --> specix:proof --> specix:audit --> specix:lens --> specix:vault
```

### Fix Path (urgent bugs)

```
With git:
specix:probe --> specix:redgreen --> specix:proof
      |
      +-- Small fix --> commit, done
      +-- Large fix --> specix:lens --> specix:branch --> specix:vault

Without git:
specix:probe --> specix:redgreen --> specix:proof
      |
      +-- Small fix --> done
      +-- Large fix --> specix:lens --> specix:vault
```

### Direct (trivial changes)

```
Make the change --> specix:proof (optional)
```

### Explore Mode (independent)

```
specix:spark (light mode) --> pure thinking, no files
      |
      +-- User satisfied --> done
      +-- User wants to build --> enter Full or Lean path (deep mode)
```

## Priority Hierarchy

```
  1. Explicit user instructions        <-- always wins
  2. Module discipline (Iron/Clay)     <-- overrides defaults
  3. System default behavior           <-- lowest priority
```

## Module Types

| Type | Meaning | Modules |
|------|---------|---------|
| **Iron** | Rigid. Follow exactly. No shortcuts. | `specix:redgreen`, `specix:probe`, `specix:proof`, `specix:grid` |
| **Clay** | Flexible. Adapt to context. | `specix:spark`, `specix:canvas` |
| **Flow** | Pipeline. Execute in order. | `specix:draft`, `specix:build`, `specix:audit`, `specix:vault`, `specix:lens`, `specix:isolate`, `specix:branch` |

## Trigger Mechanisms

| Method | Example |
|--------|---------|
| **Auto-detect** | Gateway matches user intent to module trigger conditions |
| **Slash command** | `/specix:draft add user authentication` |
| **Chain** | `specix:build` decomposes tasks then calls `specix:grid` |

## Session Resumption

Check `.specix/changes/` for active work. Infer progress from disk state:

| Disk State | Resume Point |
|------------|-------------|
| Only `charter.md` exists | Return to `specix:draft` for remaining artifacts |
| All artifacts present, lineup unchecked | Ready for `specix:build` |
| Lineup has unchecked items | Continue `specix:build` |
| All checkboxes checked | Proceed to `specix:proof` --> `specix:audit` |

## Self-Learning

`.specix/learn/insights.jsonl` accumulates session-level observations:

- Patterns that worked well (or didn't)
- Project-specific conventions discovered during exploration
- Recurring pitfalls to watch for

Gateway reads this file at startup and incorporates recent insights into routing decisions. New insights are appended during `specix:vault` archival.

## Multiple Active Changes

When `.specix/changes/` contains more than one subfolder:

- User specified a name --> use that change
- Only one change exists --> auto-select
- Multiple, no specification --> list all with progress, ask user to pick

## Activation

On every session start, read this file. Then:

1. Detect git availability
2. Scan `.specix/learn/insights.jsonl` for recent learnings (if exists)
3. Scan `.specix/changes/` for active work (if exists)
4. Present a brief status summary to the user (include git status)
5. Wait for user input, then route accordingly
