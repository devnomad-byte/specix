<div align="center">

# Specix

**Spec-driven development for Claude Code**

[中文](./README_CN.md) | English

Define before you build. Verify before you ship.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

</div>

---

## Why Specix?

Most wasted effort in software comes not from writing code slowly,
but from writing the **wrong code quickly**.
Specix structures your workflow so you solve the right problem,
build the right solution, and verify with real evidence — not hope.

Three core beliefs:

- **Structure Before Action** — spec documents are the source of truth, not an afterthought
- **Evidence Before Claims** — "it should work" is the most dangerous phrase in engineering
- **Separation of Concerns** — different questions answered at different stages by different modules

---

## Highlights

**Spec-Driven Artifacts**
Every change produces four documents — charter (why), deltas (what), blueprint (how), lineup (when). These are the source of truth, not conversation memory. Close your terminal, come back tomorrow, Specix picks up exactly where you left off.

**Four Verification Layers**
Progressive verification where each layer answers a different question: Did this task follow the spec? Does the code run? Does it match the spec and solve the real problem? Is it well-crafted and secure?

**Git-Optional**
12 of 14 modules work without git. Local development, prototypes, learning projects — all get the full spec-driven workflow. Only `isolate` (worktree) and `branch` (integration) require git.

**Executable Lineup**
Tasks include exact file paths, complete code blocks, and runnable verify commands. No "TBD", no "fill in later", no "similar to X". Every step is zero-ambiguity and directly implementable.

**Cross-Session Recovery**
State lives on disk in `.specix/`, not in chat history. A partially completed lineup is a resumable checkpoint — the next session reads the checkboxes and continues.

**Self-Learning**
`insights.jsonl` accumulates patterns, pitfalls, and project-specific conventions across sessions. Gateway reads these on startup and improves routing decisions over time.

**Anti-AI-Default Canvas**
The frontend design module refuses generic patterns — purple gradients, Inter/Roboto fonts, pill buttons, "clean and modern" as a goal. Every generation commits to a deliberate aesthetic direction.

**Dual Triggering**
Natural language matching via module descriptions, or explicit slash commands like `/specix:build`. Describe what you want in plain words, or invoke by name.

---

## The Workflow

```
Idea ──► spark ──► draft ──► build ──► proof ──► audit ──► lens ──► branch ──► vault
 explore    charter    lineup    run      verify    spec      review    integrate   archive
            deltas              tasks    evidence  check     quality
            blueprint
```

1. **Explore** — `specix:spark` Socratic questioning to clarify and confirm direction
2. **Specify** — `specix:draft` produces charter, deltas, blueprint, and lineup
3. **Build** — `specix:build` validates lineup, dispatches workers through `specix:grid`
4. **Verify** — `specix:proof` runs commands and demands fresh evidence
5. **Audit** — `specix:audit` dual-layer check: spec compliance + real-need traceback
6. **Review** — `specix:lens` holistic code quality with specialist lenses
7. **Integrate** — `specix:branch` merge, PR, keep, or discard
8. **Archive** — `specix:vault` merges deltas into master specs, records learnings

---

## Module Map

14 modules across 4 layers. All called with `specix:` prefix.

### Flow — Orchestrating the Pipeline

| Module | Purpose |
|--------|---------|
| [`specix:gateway`](./skills/gateway/SKILL.md) | Session bootstrap — detects environment, selects development path |
| [`specix:draft`](./skills/flow/draft/SKILL.md) | Produces all specification artifacts: charter, deltas, blueprint, lineup |
| [`specix:build`](./skills/flow/build/SKILL.md) | Validates lineup is executable, dispatches workers via grid |
| [`specix:audit`](./skills/flow/audit/SKILL.md) | Dual-layer verification: spec compliance + real-need traceback |
| [`specix:vault`](./skills/flow/vault/SKILL.md) | Archives change, merges deltas into master specs |

### Craft — Shaping the Work

| Module | Purpose |
|--------|---------|
| [`specix:spark`](./skills/craft/spark/SKILL.md) | Exploration + Socratic ideation (light mode / deep mode) |
| [`specix:probe`](./skills/craft/probe/SKILL.md) | Systematic debugging: root cause → pattern → hypothesis → fix |
| [`specix:proof`](./skills/craft/proof/SKILL.md) | Iron gate — run commands, read output, confirm before claiming done |

### Engine — Executing the Build

| Module | Purpose |
|--------|---------|
| [`specix:grid`](./skills/engine/grid/SKILL.md) | Multi-agent parallel dispatch with per-task review |
| [`specix:redgreen`](./skills/engine/redgreen/SKILL.md) | Test-driven discipline: failing test → implement → refactor |
| [`specix:lens`](./skills/engine/lens/SKILL.md) | Whole-change code review + specialist lenses (security / performance / architecture) |
| [`specix:branch`](./skills/engine/branch/SKILL.md) | Branch lifecycle: merge, PR, keep, or discard *(git required)* |
| [`specix:isolate`](./skills/engine/isolate/SKILL.md) | Git worktree isolation for feature work *(git required)* |

### Studio — Domain-Specific Guidance

| Module | Purpose |
|--------|---------|
| [`specix:canvas`](./skills/studio/canvas/SKILL.md) | Front-end design with anti-default aesthetics (on-demand) |

---

## Development Paths

Gateway detects your environment and picks the right path automatically.

```
User request
    │
    ▼
[specix:gateway] evaluates scope
    │
    ├─ Large change  ──► Full path
    ├─ Medium change ──► Lean path
    ├─ Bug fix       ──► Fix path
    └─ Trivial edit  ──► Direct
```

### With Git

```
Full:   spark → draft → isolate → build → proof → audit → lens → branch → vault
Lean:   draft → build → proof → audit → lens → branch → vault
Fix:    probe → redgreen → proof → (lens + branch if large)
```

### Without Git

```
Full:   spark → draft → build → proof → audit → lens → vault
Lean:   draft → build → proof → audit → lens → vault
Fix:    probe → redgreen → proof → (lens if large)
```

### Independent Use

```
Explore:  specix:spark (light mode) — anytime, pure thinking, no files
Direct:   just do it → specix:proof (optional)
```

---

## How It Works

### The Four Verification Layers

Every change goes through progressive verification — each layer answers a different question:

| Layer | Module | Question |
|-------|--------|----------|
| Micro | `grid` reviewer | Did this task implement the spec correctly? |
| Functional | `proof` | Does the code actually run? |
| Semantic | `audit` | Does it match the spec? Does it solve the real problem? |
| Quality | `lens` | Is the code well-crafted? Secure? Performant? |

### Specification Artifacts

Each change produces four artifacts in `.specix/changes/<name>/`:

```
charter.md      → Why: motivation, scope, success criteria
deltas/*.md     → What: incremental spec changes (NEW / EDIT / DROP / RENAME)
blueprint.md    → How: technical design, key decisions
lineup.md       → When: ordered task checklist with executable steps
```

### Self-Learning

Specix records observations in `.specix/learn/insights.jsonl` during archival.
Over time it accumulates what worked, what didn't, and project-specific conventions.
Gateway reads these insights at startup and improves routing decisions.

---

## Installation

### npm (recommended)

```bash
npm install specix
```

### Project-level

Clone or copy into your project, then add to `.claude/settings.json`:

```json
{ "enabledPlugins": { "specix": true } }
```

### Global

Place in `~/.claude/plugins/` and enable in user-level settings.

---

## Configuration

Specix reads `.specix/project.yaml` in your project root. Created automatically on first `specix:draft`.

```yaml
schema: spec-driven

context: |
  Stack: Node.js + TypeScript + React
  Database: PostgreSQL
  Testing: Vitest

rules:
  charter:
    - Include performance impact assessment
  deltas:
    - All endpoints must note auth requirements
  blueprint:
    - Database changes must include migration scripts
  lineup:
    - Each task must include a verification step
```

---

## Module Types

| Type | Meaning | Modules |
|------|---------|---------|
| **Iron** | Rigid. Follow exactly. No shortcuts. | `proof`, `probe`, `redgreen`, `grid` |
| **Clay** | Flexible. Adapt to context. | `spark`, `canvas` |
| **Flow** | Pipeline. Execute in order. | `draft`, `build`, `audit`, `vault`, `lens`, `branch`, `isolate` |

---

## License

[MIT](./LICENSE)
