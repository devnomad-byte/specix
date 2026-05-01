<div align="center">

# ⬡ Specix

**Spec-driven development for Claude Code**

[中文](./README_CN.md) | English

**Define before you build. Verify before you ship.**

```bash
claude plugin marketplace add devnomad-byte/specix
claude plugin install specix@devnomad-byte-specix
```

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Modules](https://img.shields.io/badge/modules-14-blueviolet.svg)](./skills/)
[![Layers](https://img.shields.io/badge/verification_layers-4-9cf.svg)](./README.md#how-it-works)

</div>

---

## 🎯 Why Specix?

> Most wasted effort in software comes not from writing code slowly,
> but from writing the **wrong code quickly**.

Three core beliefs:

- 🏗️ **Structure Before Action** — spec documents are the source of truth, not an afterthought
- 🔬 **Evidence Before Claims** — "it should work" is the most dangerous phrase in engineering
- 🧩 **Separation of Concerns** — different questions answered at different stages by different modules

---

## ✨ Highlights

> 📋 **Spec-Driven Artifacts**
> Every change produces four documents — charter (why), deltas (what), blueprint (how), lineup (when).
> Source of truth lives on disk, not in conversation memory.

> 🔍 **Four Verification Layers**
> Micro → Functional → Semantic → Quality. Each layer answers a different question.

> 🔓 **Git-Optional**
> 12 of 14 modules work without git. Local dev, prototypes, learning projects — full workflow, no repo required.

> 🎯 **Executable Lineup**
> Exact file paths, complete code blocks, runnable verify commands. Zero ambiguity, zero placeholders.

> 💾 **Cross-Session Recovery**
> State on disk, not in chat. Close terminal, return tomorrow, resume exactly where you left off.

> 🧠 **Self-Learning**
> `insights.jsonl` accumulates patterns and pitfalls across sessions. Gateway gets smarter over time.

> 🎨 **Anti-AI-Default Canvas**
> Frontend design module refuses generic patterns — purple gradients, Inter/Roboto, pill buttons. Deliberate aesthetics only.

> ⚡ **Dual Triggering**
> Natural language matching via descriptions, or `/specix:build` slash commands. Your choice.

---

## 🔄 The Workflow

```
                        ┌──────────────────────────────────────────────────────────────────────┐
                        │                       Specix Full Pipeline                          │
                        └──────────────────────────────────────────────────────────────────────┘

   ┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐
   │         │     │         │     │         │     │         │     │         │     │         │     │         │     │         │
   │  spark  │────►│  draft  │────►│  build  │────►│  proof  │────►│  audit  │────►│  lens   │────►│ branch  │────►│  vault  │
   │         │     │         │     │         │     │         │     │         │     │         │     │         │     │         │
   └─────────┘     └─────────┘     └─────────┘     └─────────┘     └─────────┘     └─────────┘     └─────────┘     └─────────┘
    EXPLORE         SPECIFY          EXECUTE          VERIFY           AUDIT           REVIEW         INTEGRATE        ARCHIVE
   Socratic QA    4 artifacts     grid dispatch    fresh evidence   spec + need     code quality     merge/PR       delta merge
   light + deep   charter/deltas   workers+review   run commands     traceback       specialist       lifecycle       self-learn
                   bluep./lineup                    read output      3-level                                         insights
```

---

## 📦 Module Map

**14 modules** across **4 layers**. All called with `specix:` prefix.

### 🌊 Flow — Orchestrating the Pipeline

| Module | Type | Purpose |
|--------|:----:|---------|
| [`specix:gateway`](./skills/gateway/SKILL.md) | Flow | Session bootstrap — detects environment, selects development path |
| [`specix:draft`](./skills/flow/draft/SKILL.md) | Flow | Produces all specification artifacts: charter, deltas, blueprint, lineup |
| [`specix:build`](./skills/flow/build/SKILL.md) | Flow | Validates lineup is executable, dispatches workers via grid |
| [`specix:audit`](./skills/flow/audit/SKILL.md) | Flow | Dual-layer verification: spec compliance + real-need traceback |
| [`specix:vault`](./skills/flow/vault/SKILL.md) | Flow | Archives change, merges deltas into master specs |

### 🔨 Craft — Shaping the Work

| Module | Type | Purpose |
|--------|:----:|---------|
| [`specix:spark`](./skills/craft/spark/SKILL.md) | Clay | Exploration + Socratic ideation (light mode / deep mode) |
| [`specix:probe`](./skills/craft/probe/SKILL.md) | Iron | Systematic debugging: root cause → pattern → hypothesis → fix |
| [`specix:proof`](./skills/craft/proof/SKILL.md) | Iron | Iron gate — run commands, read output, confirm before claiming done |

### ⚡ Engine — Executing the Build

| Module | Type | Purpose |
|--------|:----:|---------|
| [`specix:grid`](./skills/engine/grid/SKILL.md) | Iron | Multi-agent parallel dispatch with per-task review |
| [`specix:redgreen`](./skills/engine/redgreen/SKILL.md) | Iron | Test-driven discipline: failing test → implement → refactor |
| [`specix:lens`](./skills/engine/lens/SKILL.md) | Flow | Whole-change code review + specialist lenses (security / performance / architecture) |
| [`specix:branch`](./skills/engine/branch/SKILL.md) | Flow | Branch lifecycle: merge, PR, keep, or discard *(git required)* |
| [`specix:isolate`](./skills/engine/isolate/SKILL.md) | Flow | Git worktree isolation for feature work *(git required)* |

### 🎨 Studio — Domain-Specific Guidance

| Module | Type | Purpose |
|--------|:----:|---------|
| [`specix:canvas`](./skills/studio/canvas/SKILL.md) | Clay | Front-end design with anti-default aesthetics (on-demand) |

### Module Types

| Type | Icon | Meaning | Modules |
|------|:----:|---------|---------|
| **Iron** | 🛡️ | Rigid. Follow exactly. No shortcuts. | `proof` `probe` `redgreen` `grid` |
| **Clay** | 🏺 | Flexible. Adapt to context. | `spark` `canvas` |
| **Flow** | 🌊 | Pipeline. Execute in order. | `draft` `build` `audit` `vault` `lens` `branch` `isolate` |

---

## 🛤️ Development Paths

Gateway detects your environment and picks the right path automatically.

```
User request
    │
    ▼
[specix:gateway] evaluates scope
    │
    ├─ 🔴 Large change  ──► Full path
    ├─ 🟡 Medium change ──► Lean path
    ├─ 🟠 Bug fix       ──► Fix path
    └─ 🟢 Trivial edit  ──► Direct
```

### With Git

```bash
Full:   spark → draft → isolate → build → proof → audit → lens → branch → vault
Lean:   draft → build → proof → audit → lens → branch → vault
Fix:    probe → redgreen → proof → (lens + branch if large)
```

### Without Git

```bash
Full:   spark → draft → build → proof → audit → lens → vault
Lean:   draft → build → proof → audit → lens → vault
Fix:    probe → redgreen → proof → (lens if large)
```

### Independent Use

```bash
Explore:  specix:spark (light mode) — anytime, pure thinking, no files
Direct:   just do it → specix:proof (optional)
```

---

## ⚙️ How It Works

### 🔍 The Four Verification Layers

Every change goes through progressive verification — each layer answers a different question:

```
  ┌─────────────────────────────────────────────────────┐
  │  Quality   │  lens        │  Is the code good?      │
  ├─────────────────────────────────────────────────────┤
  │  Semantic   │  audit       │  Does it solve the      │
  │             │              │  real problem?           │
  ├─────────────────────────────────────────────────────┤
  │  Functional │  proof       │  Does it run?            │
  ├─────────────────────────────────────────────────────┤
  │  Micro      │  grid reviewer│ Did this task follow    │
  │             │              │  the spec?               │
  └─────────────────────────────────────────────────────┘
```

### 📋 Specification Artifacts

Each change produces four artifacts in `.specix/changes/<name>/`:

```
charter.md      → Why: motivation, scope, success criteria
deltas/*.md     → What: incremental spec changes (NEW / EDIT / DROP / RENAME)
blueprint.md    → How: technical design, key decisions
lineup.md       → When: ordered task checklist with executable steps
```

### 🧠 Self-Learning

Specix records observations in `.specix/learn/insights.jsonl` during archival.
Over time it accumulates what worked, what didn't, and project-specific conventions.
Gateway reads these insights at startup and improves routing decisions.

---

## 📥 Installation

Three ways to install — pick the one that fits you.

### 1. Plugin Marketplace *(recommended)*

Add this repo as a marketplace source, then install:

```
/plugin marketplace add devnomad-byte/specix
/plugin install specix@devnomad-byte-specix
```

Or from terminal:

```bash
claude plugin marketplace add devnomad-byte/specix
claude plugin install specix@devnomad-byte-specix
```

### 2. Official Community *(coming soon)*

Once published to the Anthropic official marketplace:

```
/plugin install specix@claude-plugins-official
```

### 3. Manual Clone *(most reliable)*

```bash
git clone https://github.com/devnomad-byte/specix.git
```

Then add to your project's `.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "specix": "/absolute/path/to/specix"
  }
}
```

Or globally in `~/.claude/settings.json`.

### Scopes

| Scope | Effect |
|-------|--------|
| `--scope user` 🏠 | Available in all your projects (default) |
| `--scope project` 👥 | Shared with collaborators via `.claude/settings.json` |
| `--scope local` 🔒 | Only for you in this repo |

---

## 🔧 Configuration

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

<div align="center">

[MIT License](./LICENSE) · Made with ⬡ for Claude Code

</div>
