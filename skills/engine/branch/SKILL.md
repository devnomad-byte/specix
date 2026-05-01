---
name: branch
description: Use after lens review passes — manages branch integration with four options for completing development work
---

# Branch: Branch Lifecycle Management

**Module type:** Flow (sequential)
**Trigger:** After specix:lens review passes (all Critical issues resolved).
**Alias prefix:** `specix:branch`
**Prerequisite:** Git must be available. If the project has no git repository, this module is unavailable — skip directly to `specix:vault`.

---

## Purpose

Branch handles the final step of development: deciding how the completed work integrates with the rest of the project. It presents clear options and executes the chosen path cleanly.

---

## Pre-Flight Verification

Before presenting options, confirm the prerequisites:

1. **Run specix:proof one final time** — all tests pass, type checks clean, linter satisfied
2. **Determine the base branch** — the branch this work should integrate into (usually `main` or `develop`)
3. **Check branch status** — are there uncommitted changes? Is the branch up to date with base?

If any prerequisite fails, resolve it before proceeding.

---

## The Four Options

Present these options to the user. Each has a clear tradeoff.

### Option A: Merge

Merge the feature branch directly into the base branch.

```
When to choose:
- Solo developer or small team with direct merge privileges
- Change is small and low-risk
- No code review policy requiring PRs

What happens:
1. Switch to base branch
2. Pull latest changes
3. Merge feature branch (resolve conflicts if any)
4. Run specix:proof on the merged result
5. Delete feature branch
6. Switch back to base branch
```

### Option B: Pull Request

Create a pull request for team review before integration.

```
When to choose:
- Team has a PR review policy
- Change is large enough to warrant peer review
- CI/CD pipeline requires PR as the integration trigger

What happens:
1. Push feature branch to remote (if not already pushed)
2. Create PR with:
   - Title: concise summary of the change
   - Body: charter summary + key decisions + test evidence
   - Labels: appropriate for the project's PR taxonomy
3. Report PR URL to the user
4. Feature branch is preserved until PR is merged
```

### Option C: Keep

Keep the feature branch without merging. Work remains on the branch.

```
When to choose:
- Not ready to integrate yet (waiting on another change, deployment window, approval)
- Want to preserve the work but continue other tasks
- Need to verify something in a different context before merging

What happens:
1. Ensure all changes are committed
2. Report current branch name and commit hash
3. Remind user that the branch can be merged later
```

### Option D: Discard

Throw away the feature branch and all its changes.

```
When to choose:
- The approach was wrong and needs to be restarted
- The requirement was cancelled
- The branch was an experiment that did not pan out

What happens (AFTER explicit user confirmation):
1. Switch to base branch
2. Delete feature branch
3. Confirm deletion
4. Report that no trace of the branch remains locally
```

---

## Execution Rules

- Never merge or discard without explicit user confirmation
- Always run specix:proof after a merge to verify the integrated result
- Preserve the charter and all spec files in `.specix/` even after merge (they are part of the project record)
- If the merge creates conflicts, present the conflict list to the user before attempting resolution

---

## Cleanup

After the chosen option is executed:

- Report the final state (which branch you are on, what happened to the feature branch)
- Remind the user if spec files in `.specix/changes/` should be archived (via specix:vault, the archive module)
- Confirm there are no stale artifacts or uncommitted files

---

## Output

```
BRANCH LIFECYCLE COMPLETE

Option chosen: [A/B/C/D]
Feature branch: [name]
Base branch: [name]
Current branch: [name]

Status:
- [What was done]
- [Any follow-up actions for the user]

Next step: specix:vault to archive the change artifacts (if applicable).
```
