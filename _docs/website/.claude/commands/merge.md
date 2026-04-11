---
description: Ship code - create PR, implement feedback, merge, cleanup. The complete "ship it" command.
---

**CRITICAL: This is a COMPLETE workflow. DO NOT exit until code is merged.**

**YOU MUST complete all 7 phases:**

1. Preparation & Validation
2. PR Creation (or verify existing)
3. Wait for Checks & Claude Review
4. Implement Feedback
5. Document Deferred Items & Lessons
6. Final Verification & Confirmation
7. Merge & Cleanup

**The workflow ends when code is merged to main - not before.**

---

**MANDATORY: Use TodoWrite to create these 7 phases as tasks BEFORE starting:**

```markdown
TodoWrite:

1. Phase 1: Preparation & Validation (pending)
2. Phase 2: PR Creation (pending)
3. Phase 3: Wait for Checks & Review (pending)
4. Phase 4: Implement Feedback (pending)
5. Phase 5: Document Deferred & Lessons (pending)
6. Phase 6: Final Verification & Confirmation (pending)
7. Phase 7: Merge & Cleanup (pending)
```

Mark each as `in_progress` when starting, `completed` when done.
**Do NOT exit until all 7 phases show `completed`.**

---

**Expected Duration: 20-35 minutes total**

- Phase 1-2: ~5 minutes
- Phase 3: 2-12 minutes (intelligent polling)
- Phase 4: ~5-10 minutes (implementing changes)
- Phase 5: ~2-3 minutes (documentation)
- Phase 6: ~2 minutes (verification + confirmation)
- Phase 7: ~1 minute (merge + cleanup)

**This is normal. Do not exit early.**

---

## Phase 1: Preparation & Validation

**Mark task as `in_progress`**

### Branch Check

```bash
BRANCH=$(git branch --show-current)
echo "Current branch: $BRANCH"
```

**If on main:** Create feature branch first or ask what to name it.

### Safety Check

```bash
git status --short
```

**Review output:**

- **If ANY deletions (D):** STOP and ask "You're about to delete X files. Is this intentional?"
- Wait for confirmation if deletions present

### Run Pre-Commit Checks

```bash
pnpm format
pnpm lint
pnpm type-check
pnpm test
pnpm build:check
```

Fix any issues automatically where possible.

### Stage All Changes

**CRITICAL: After running checks, stage ALL changes with `git add -A`**

- NEVER use selective staging
- `pnpm format` may modify files - these MUST be included
- This has caused lost work in the past

### Verify Staging

```bash
git status --short
```

All files should show as staged (M/A, not " M").

**Mark task as `completed`**

---

## Phase 2: PR Creation

**Mark task as `in_progress`**

### Check for Existing PR

```bash
gh pr view --json number,title,state 2>/dev/null
```

**If PR exists and state is OPEN:** Skip to Phase 3.

**If PR exists and state is MERGED:** Report "Already merged" and exit.

**If no PR exists:** Continue with creation.

### Commit Changes

```bash
git add -A
git commit -m "[type]: [description]

Generated with [Claude Code](https://claude.ai/code)
via [Happy](https://happy.engineering)

Co-Authored-By: Claude <noreply@anthropic.com>
Co-Authored-By: Happy <yesreply@happy.engineering>"
```

### Push Branch

```bash
git push -u origin $(git branch --show-current)
```

### Create PR

```bash
gh pr create --title "[type]: [description]" --body "## Summary

[Brief description of changes]

## Changes

- [List key changes]

## Test Plan

- [ ] All pre-commit checks pass
- [ ] Manual testing completed

---

Generated with [Claude Code](https://claude.ai/code)
via [Happy](https://happy.engineering)"
```

Capture PR number from output.

**Mark task as `completed`**

---

## Phase 3: Wait for Checks & Claude Review

**Mark task as `in_progress`**

**CRITICAL: You MUST wait and poll for checks. Do NOT skip this phase.**

### Intelligent Polling (2-12 minutes)

1. **Initial wait:** 2 minutes (allow checks to start)

2. **Poll every 30 seconds** (max 12 minutes = 24 polls):

```bash
gh pr checks [PR_NUMBER] --json name,status,conclusion
```

**Required checks:**

- **Claude:** name contains "claude" (case-insensitive)
- **Vercel Deploy:** name contains "vercel" AND ("deploy" OR "production")
- **Vercel Preview:** name contains "vercel" AND "preview"

3. **Check completion:**
   - If any required check status != "completed" → continue polling
   - If all completed → evaluate conclusions

4. **Evaluate conclusions:**
   - **Vercel Deploy failure:** BLOCKING ERROR - stop and report build errors
   - **Claude Review failure:** WARNING - may have partial feedback
   - **Vercel Preview failure:** WARNING - non-blocking

5. **On Vercel deployment failure:**
   - Stop polling
   - Report: "BLOCKING: Vercel deployment failed. Build errors must be fixed."
   - Exit workflow

### Get Claude's Review Comment

```bash
gh pr view [PR_NUMBER] --json comments | jq '[.comments[] | select(.author.login | ascii_downcase | test("claude"))] | sort_by(.updatedAt // .createdAt) | reverse | .[0].body // ""'
```

**If comment exists and length > 100 chars:**

- Extract sections:
  - "Immediate Changes (Critical Fixes)"
  - "Important Changes for Later"
  - "Software Engineering Lesson"
- Proceed to implementation

**If no comment after timeout:**

- Proceed with warning: "Claude review not found after 12 minutes"

**Mark task as `completed`**

---

## Phase 4: Implement Feedback

**Mark task as `in_progress`**

### Process Immediate Changes

For each item in Claude's "Immediate Changes (Critical Fixes)":

1. **Implement the change** following project patterns
2. **Run checks:**
   ```bash
   pnpm format
   pnpm lint
   pnpm type-check
   pnpm test
   pnpm build:check
   ```
3. **Commit:**

   ```bash
   git add -A
   git commit -m "fix: address review feedback - [brief description]

   Generated with [Claude Code](https://claude.ai/code)
   via [Happy](https://happy.engineering)

   Co-Authored-By: Claude <noreply@anthropic.com>
   Co-Authored-By: Happy <yesreply@happy.engineering>"
   ```

4. **Push:**
   ```bash
   git push
   ```

### Update PR

```bash
gh pr comment [PR_NUMBER] --body "## Feedback Addressed

- [x] [Item 1]: Implemented
- [x] [Item 2]: Implemented

Ready for merge."
```

**Mark task as `completed`**

---

## Phase 5: Document Deferred Items & Lessons

**Mark task as `in_progress`**

### Document Deferred Tasks

Append to `_docs/roadmaps/pr-feedback-tasks.md`:

For each item in "Important Changes for Later":

- Create task entry with: title, value, complexity, context, PR link
- Format as markdown task list

### Document Lesson

If Claude included "Software Engineering Lesson":

Create file: `_docs/lessons/YYYY-MM-DD-pr-[NUMBER]-[topic].md`

```markdown
# [Lesson Title]

**PR:** #[NUMBER]
**Date:** YYYY-MM-DD

## Lesson

[Content from Claude's review]

## Application

[How this applies to the codebase]
```

**Mark task as `completed`**

---

## Phase 6: Final Verification & Confirmation

**Mark task as `in_progress`**

### Re-check All Checks

```bash
gh pr checks [PR_NUMBER] --json name,conclusion
```

All required checks must pass.

### Verify Clean State

```bash
git status --short
```

Should show no uncommitted changes.

### Get Merge Readiness

```bash
gh pr view [PR_NUMBER] --json mergeable,mergeStateStatus
```

### Present Summary

```markdown
## Ready to Merge: PR #[NUMBER]

**Title:** [title]
**Branch:** [branch] -> main

### Checks

- [ ] Vercel Deploy: [status]
- [ ] Claude Review: [status]
- [ ] All tests: [status]

### Feedback

- **Implemented:** [X] immediate changes
- **Deferred:** [Y] items to roadmap
- **Lesson:** [documented/none]

### Changes

- Files: [N]
- Lines: +[X] / -[Y]

---

**Ready to merge and delete branch?**
```

### Confirm with User

```markdown
AskUserQuestion:
question: "Merge PR #[NUMBER] and delete branch?"
header: "Merge"
options: - label: "Yes, merge and delete branch"
description: "Squash merge to main, delete feature branch" - label: "Merge only (keep branch)"
description: "Merge but preserve the branch" - label: "Wait - review in browser first"
description: "Open PR in browser for manual review"
```

**Mark task as `completed`**

---

## Phase 7: Merge & Cleanup

**Mark task as `in_progress`**

### On "Yes, merge and delete branch"

```bash
gh pr merge [PR_NUMBER] --squash --delete-branch
```

### On "Merge only (keep branch)"

```bash
gh pr merge [PR_NUMBER] --squash
```

### On "Wait - review in browser first"

```bash
gh pr view [PR_NUMBER] --web
```

Report: "Opened PR in browser. Run /merge again when ready."
STOP workflow.

### Return to Main

```bash
git checkout main
git pull origin main
```

### Verify Merge

```bash
gh pr view [PR_NUMBER] --json state
```

State should be "MERGED".

**Mark task as `completed`**

---

## Final Report

```markdown
## Merge Complete

**PR #[NUMBER]:** [title]
**Merged to:** main
**Branch deleted:** [yes/no]

### Summary

- [x] immediate changes implemented
- [Y] items deferred to roadmap
- [Z] lesson documented
- All checks passed
- Squash merged to main

### What Shipped

- [Key change 1]
- [Key change 2]

### Next Steps

- View: `git log -1`
- Deferred: `_docs/roadmaps/pr-feedback-tasks.md`
- New feature: `git checkout -b feature/[name]`

**Ship complete.**
```

---

## Error Handling

### Merge Conflicts

```bash
git fetch origin main
git rebase origin/main
# Resolve conflicts
git rebase --continue
git push --force-with-lease
```

Run `/merge` again after resolving.

### Failed Checks After Implementation

Fix issues and push again. Checks will re-run automatically.

### PR Already Merged

Report current state and exit - nothing to do.

---

## Workflow Integration

**Complete development cycle:**

```
/plan -> /work -> /review -> /merge
   ^                            |
   |____________________________|
```

**What each command does:**

- `/plan` - Design implementation approach
- `/work` - Execute implementation with iteration
- `/review` - Multi-agent code review with auto-fix
- `/merge` - Ship: PR, feedback, merge, cleanup

---

## Flexibility

**"Skip feedback implementation"**
→ Go straight to merge confirmation (document skipped items)

**"Don't wait for Claude"**
→ Skip Phase 3 polling, proceed to merge

**"Create PR only"**
→ Stop after Phase 2 (equivalent to old /pr behavior)

---

## Remember

**Human-in-the-loop for merge decision.**

Your job:

- Run all checks
- Create/verify PR
- Wait for and implement feedback
- Document deferred items
- Get confirmation before merge
- Execute merge and cleanup

NOT your job:

- Auto-merge without confirmation
- Skip failed checks
- Leave feedback unaddressed
- Forget documentation

**This is the "ship it" command. It ends when code is on main.**
