---
name: vault
description: Use when a change is fully complete and verified — merges delta specs into master specs and archives the change folder
---

# specix:vault — Archival Engine

You finalize a completed change. Your job: merge delta specifications into the master specs, verify integrity, then seal the change into the archive.

## When to Activate

- `specix:audit` and `specix:lens` both passed
- User confirms the change is done and should be archived
- User types `/specix:vault`
- Gateway detects all verification steps completed

## Process Overview

```
1. PRE-MERGE CHECK
       |
       v
2. MERGE DELTAS (strict order: RENAME > DROP > EDIT > NEW)
       |
       v
3. POST-MERGE VERIFY
       |
       v
4. ARCHIVE
       |
       v
5. LEARN (append insights)
```

## Step 1 — Pre-Merge Check

Before touching any master spec, scan for problems:

- **Duplicate requirement names**: Two NEW blocks targeting the same name across different delta files
- **Cross-section conflicts**: A NEW block references a section that another delta is DROPping
- **Orphan references**: A delta references a requirement that doesn't exist in master specs and isn't NEW'd elsewhere

**Resolution:**

| Issue | Action |
|-------|--------|
| Duplicate name | Pause. Report both deltas. Ask user which takes precedence. |
| Cross-section conflict | Pause. Show the conflict. Ask user for resolution. |
| Orphan reference | Warn but continue. Log the orphan for manual review. |

Only proceed when all blocking issues are resolved.

## Step 2 — Merge Deltas

Read each file in `.specix/changes/<name>/deltas/`. For every operation block, apply to the corresponding master spec in `.specix/specs/`.

**Execution order is non-negotiable:**

```
1. RENAME operations first (update names so subsequent ops target correct blocks)
2. DROP operations second  (remove before editing to avoid conflicts)
3. EDIT operations third    (modify existing content)
4. NEW operations last      (append fresh content)
```

### RENAME Operation

```
#### RENAME: OldName → NewName
```
- Locate `#### OldName` in the target master spec
- Replace with `#### NewName`
- Subsequent EDIT/DROP in the same delta session will reference NewName

### DROP Operation

```
#### DROP: RequirementName
```
- Locate the requirement block in master spec
- Remove the entire block (from `####` heading to the next `####` or end of section)
- If not found: **skip this operation, log a warning, continue**

### EDIT Operation

```
#### EDIT: RequirementName
New content replacing the old block.
```
- Locate the matching requirement block in master spec
- Replace the entire block content with the new content
- If not found: **skip, log warning, continue**

### NEW Operation

```
#### NEW: RequirementName
Content for the new requirement.
```
- Identify the target section (from the delta file's scope)
- Append the new requirement block at the end of that section
- If the target section doesn't exist in master spec: **create the section, then append**

## Step 3 — Post-Merge Verify

After all deltas are merged:

1. **Structure check**: Every master spec file has valid headings, no broken markdown
2. **Reference check**: No requirement name is referenced that doesn't exist
3. **Completeness check**: No empty sections or placeholder text left behind

If verification fails, report the issue but do NOT revert the merge. The user must inspect manually.

## Step 4 — Archive

Move the entire change directory to the archive:

```
.specix/changes/<name>/  -->  .specix/archive/YYYY-MM-DD-<name>/
```

The archive preserves the full change history: charter, deltas, blueprint, lineup — all untouched.

## Step 5 — Learn

Open `.specix/learn/insights.jsonl`. Append a JSON line summarizing what was learned during this change:

```json
{
  "date": "YYYY-MM-DD",
  "change": "<name>",
  "patterns": ["what worked", "what to repeat"],
  "pitfalls": ["what went wrong", "what to avoid"],
  "conventions": ["project-specific discoveries"]
}
```

This file grows over time. `specix:gateway` reads it at session start to improve routing.

## Exception Summary

| Exception | Handling |
|-----------|----------|
| EDIT/DROP target not found in master spec | Skip operation, log warning, continue merge |
| NEW target section doesn't exist | Auto-create section, then append |
| Merge conflict (same requirement targeted by multiple ops) | Pause merge, report to user, request manual resolution |
| Master spec file doesn't exist | Create it, then apply NEW operations normally |

## After Completion

Confirm archival:

> "Change `<name>` archived to `.specix/archive/YYYY-MM-DD-<name>/`. Delta specs merged into master. Insights recorded."

The change is now sealed. Master specs reflect the new state of the project.
