---
description: Multi-agent code review with auto-fix - spawn parallel reviewers, fix clear issues, escalate decisions
---

**MANDATORY FIRST STEP: Create task list with TodoWrite BEFORE starting.**

You are the orchestrator running a comprehensive code review. Spawn specialized reviewers in parallel, aggregate findings, auto-fix clear issues, and present decisions to Craig.

---

## Workflow Overview

1. **Create task list** (TodoWrite)
2. **Gather context** (plan, diff, test status)
3. **Spawn 4 reviewers in parallel** (security, performance, architecture, correctness)
4. **Aggregate & categorize findings** (AUTO_FIX vs NEEDS_DECISION)
5. **Auto-fix clear issues** (spawn engineer)
6. **Commit report & fixes**
7. **Validate preview deployment** (browser testing on Vercel preview URL)
8. **Present decisions to Craig** (if any)
9. **Handle decisions & final report**

**Expected duration:** 20-50 min depending on scope

---

## Phase 0: Initialize Task List

**FIRST ACTION: Use TodoWrite to create task list.**

```markdown
TodoWrite:

1. Gather context (plan, diff, tests) (pending)
2. Spawn 4 reviewers in parallel (pending)
3. Aggregate findings (pending)
4. Auto-fix clear issues (pending)
5. Commit report & fixes (pending)
6. Validate preview deployment (pending)
7. Present decisions to Craig (pending)
8. Final report (pending)
```

---

## Phase 1: Gather Context (2 min)

**Mark task "Gather context" as `in_progress`**

### Detect Review Scope

```bash
# Check current branch
git branch --show-current

# If on feature branch, diff against main
git diff main...HEAD --stat

# List changed files
git diff main...HEAD --name-only
```

**If on main:** Review last commit or ask Craig what to review.

### Load Plan Context (if exists)

```bash
# Find most recent approved plan
ls -la .claude/plans/*.md | head -5
```

Read plan for:

- Success criteria
- Test specifications
- Original requirements

### Check Test Status

```bash
pnpm test
```

**Save context:**

```markdown
REVIEW_CONTEXT:
branch: [current branch]
base: main
changed_files: [list]
plan_path: [if found]
test_status: [PASS/FAIL]
```

**Mark task as `completed`**

---

## Phase 2: Parallel Review (5-15 min)

**Mark task "Spawn 4 reviewers" as `in_progress`**

### Get Diff for Reviewers

```bash
git diff main...HEAD
```

Save diff content for reviewer prompts.

### Diff Size Check

```bash
git diff main...HEAD --stat | tail -1
```

**If diff is very large (>2000 lines):**

```markdown
Large diff detected ([X] files, [Y] lines).

**Options:**

1. Review by directory - split into focused reviews
2. Review key files only - [suggest most critical files]
3. Proceed with full review - may take longer and cost more tokens

**Recommendation:** [based on file types and structure]
```

Wait for Craig's preference before proceeding with large diffs.

### Gather Project Context

**Read these files to provide codebase context to reviewers:**

```bash
# Package.json for dependencies and scripts
cat package.json | head -50

# TypeScript config for compiler options
cat tsconfig.json

# Existing patterns (if available)
ls src/features/ 2>/dev/null | head -10
```

**Project context to inject:**

```markdown
PROJECT_CONTEXT:
framework: [React/Next.js/etc from package.json]
key_dependencies: [list major libs]
test_framework: [vitest/jest/etc]
patterns_used: [feature-based structure, etc]
typescript: [strict mode, etc]
```

### Spawn All 4 Reviewers Simultaneously

**CRITICAL: All 4 agents run IN PARALLEL using a single message with 4 Task calls.**

````markdown
Task(
subagent_type: "security-reviewer",
model: "haiku",
prompt: " # Security Review

    ## Project Context
    [PROJECT_CONTEXT from above]

    ## Diff to Review
    ```diff
    [DIFF CONTENT]
    ```

    Check for:
    - OWASP Top 10 vulnerabilities
    - SQL injection, XSS, CSRF
    - Auth/authz bypass
    - Hardcoded secrets or credentials
    - Data exposure risks
    - Input validation gaps

    Output format - for each finding:
    - **[SEC-N]** [severity: CRITICAL|IMPROVEMENT|NIT]
      - Location: `file.ts:line`
      - Issue: [description]
      - Fix: [specific recommendation]
      - Auto-fixable: YES|NO
      - Reason: [why auto-fixable or not]

    If no issues found, output: 'No security issues found.'

"
)

Task(
subagent_type: "performance-reviewer",
model: "haiku",
prompt: " # Performance Review

    ## Project Context
    [PROJECT_CONTEXT from above]

    ## Diff to Review
    ```diff
    [DIFF CONTENT]
    ```

    Check for:
    - N+1 queries
    - Unnecessary re-renders (React)
    - Bundle size impact
    - Missing memoization
    - Memory leaks
    - Inefficient algorithms
    - Missing caching opportunities

    Output format - for each finding:
    - **[PERF-N]** [severity: CRITICAL|IMPROVEMENT|NIT]
      - Location: `file.ts:line`
      - Issue: [description]
      - Fix: [specific recommendation]
      - Auto-fixable: YES|NO
      - Reason: [why auto-fixable or not]

    If no issues found, output: 'No performance issues found.'

"
)

Task(
subagent_type: "architecture-reviewer",
model: "haiku",
prompt: " # Architecture Review

    ## Project Context
    [PROJECT_CONTEXT from above]

    ## Diff to Review
    ```diff
    [DIFF CONTENT]
    ```

    Check for:
    - Pattern alignment with codebase
    - Single Responsibility Principle violations
    - YAGNI violations (over-engineering)
    - Modularity issues
    - Unnecessary complexity
    - Coupling problems
    - Abstraction issues

    Output format - for each finding:
    - **[ARCH-N]** [severity: CRITICAL|IMPROVEMENT|NIT]
      - Location: `file.ts:line`
      - Issue: [description]
      - Fix: [specific recommendation]
      - Auto-fixable: YES|NO
      - Reason: [why auto-fixable or not]

    If no issues found, output: 'No architecture issues found.'

"
)

Task(
subagent_type: "correctness-reviewer",
model: "haiku",
prompt: " # Correctness Review

    ## Project Context
    [PROJECT_CONTEXT from above]

    ## Diff to Review
    ```diff
    [DIFF CONTENT]
    ```

    Check for:
    - Business logic errors
    - Edge case handling
    - Error handling completeness
    - State management issues
    - Race conditions
    - Null/undefined handling
    - Type safety gaps

    Output format - for each finding:
    - **[CORR-N]** [severity: CRITICAL|IMPROVEMENT|NIT]
      - Location: `file.ts:line`
      - Issue: [description]
      - Fix: [specific recommendation]
      - Auto-fixable: YES|NO
      - Reason: [why auto-fixable or not]

    If no issues found, output: 'No correctness issues found.'

"
)
````

**Wait for all 4 reviewers to complete.**

**Mark task as `completed`**

---

## Phase 3: Aggregate & Categorize (2 min)

**Mark task "Aggregate findings" as `in_progress`**

### Collect All Findings

Gather outputs from all 4 reviewers:

- Security findings (SEC-N)
- Performance findings (PERF-N)
- Architecture findings (ARCH-N)
- Correctness findings (CORR-N)

### Deduplicate

If same issue found by multiple reviewers:

- Keep the most detailed description
- Note which reviewers flagged it

### Categorize

**AUTO_FIX bucket:**

- `Auto-fixable: YES`
- Clear, unambiguous fix
- No trade-offs or architectural decisions

**NEEDS_DECISION bucket:**

- `Auto-fixable: NO`
- Multiple valid approaches
- Architectural implications
- Trade-offs involved

**Mark task as `completed`**

---

## Phase 4: Auto-Fix Loop (5-30 min)

**Mark task "Auto-fix clear issues" as `in_progress`**

### If No AUTO_FIX Items

Skip to Phase 5.

### If AUTO_FIX Items Exist

**Spawn engineer to fix:**

```markdown
Task(
subagent_type: "engineer",
model: "sonnet",
prompt: " # Fix Review Findings

    Apply these fixes exactly as specified:

    ## AUTO_FIX Items

    [list each finding with location and fix]

    ## Constraints

    - Apply each fix exactly as described
    - Run tests after all fixes: `pnpm test`
    - Do NOT modify any NEEDS_DECISION items
    - Report any fix that couldn't be applied

    ## Output

    Report:
    - Files modified
    - Fixes applied
    - Test results
    - Any issues encountered

"
)
```

### Verify Fixes

After engineer completes:

1. Run tests: `pnpm test`
2. Check fixes were applied correctly
3. Note any that failed

**Mark task as `completed`**

---

## Phase 5: Commit Report & Fixes (5 min)

**Mark task "Commit report & fixes" as `in_progress`**

### Generate Review Report

Create file: `.claude/reviews/YYYY-MM-DD-HHMM-[feature].md`

```markdown
# Code Review: [Feature/Branch Name]

**Date:** YYYY-MM-DD
**Branch:** [branch name]
**Plan:** [plan path if exists]
**Reviewers:** Security, Performance, Architecture, Correctness

---

## Summary

| Category     | Critical | Improvement | Nit | Auto-Fixed |
| ------------ | -------- | ----------- | --- | ---------- |
| Security     | X        | Y           | Z   | A          |
| Performance  | X        | Y           | Z   | B          |
| Architecture | X        | Y           | Z   | C          |
| Correctness  | X        | Y           | Z   | D          |
| **Total**    | X        | Y           | Z   | E          |

---

## Auto-Fixed Issues

[List of issues that were automatically fixed]

---

## Needs Decision

[List of issues requiring Craig's input]

---

## All Findings

### Security

[full findings]

### Performance

[full findings]

### Architecture

[full findings]

### Correctness

[full findings]

---

## What's Good

[Positive observations from reviewers]
```

### Safety Check (Before Commit)

```bash
git status --short
```

**Review output:**

- **If ANY deletions (D):** STOP and ask Craig "You're about to delete X files. Is this intentional?"
- Wait for confirmation before proceeding if deletions present

### Run Pre-Commit Checks

```bash
pnpm format
pnpm lint
pnpm type-check
pnpm test
pnpm build:check
```

Fix any issues that arise.

### Commit

```bash
git add -A
git commit -m "review: [feature] - X issues fixed, Y need decision

Auto-fixed:
- [list key fixes]

Needs decision:
- [list key decisions]

Generated with [Claude Code](https://claude.ai/code)
via [Happy](https://happy.engineering)

Co-Authored-By: Claude <noreply@anthropic.com>
Co-Authored-By: Happy <yesreply@happy.engineering>"

git push
```

**Mark task as `completed`**

---

## Phase 5.5: Vercel Preview Deployment Validation (3-5 min)

**Mark task "Validate preview deployment" as `in_progress`**

After pushing to remote, Vercel automatically creates a preview deployment. This phase validates the UI on the production-like preview environment.

### Prerequisites

- `VERCEL_TOKEN` environment variable set (create at vercel.com/account/tokens)
- Vercel CLI installed (`npm i -g vercel`) or use API directly

### Wait for Deployment to be Ready

```bash
# Get current commit SHA
COMMIT_SHA=$(git rev-parse HEAD)

# Poll Vercel API for deployment readiness (max 5 min)
echo "Waiting for Vercel deployment..."
for i in {1..20}; do
  # Query deployments filtered by commit SHA
  DEPLOYMENT_JSON=$(curl -s -H "Authorization: Bearer $VERCEL_TOKEN" \
    "https://api.vercel.com/v6/deployments?projectId=$VERCEL_PROJECT_ID&limit=5")

  # Find deployment matching our commit that is READY
  PREVIEW_URL=$(echo "$DEPLOYMENT_JSON" | jq -r --arg sha "$COMMIT_SHA" \
    '.deployments[] | select(.meta.githubCommitSha == $sha and .state == "READY") | .url' | head -1)

  if [ -n "$PREVIEW_URL" ] && [ "$PREVIEW_URL" != "null" ]; then
    echo "Deployment ready: https://$PREVIEW_URL"
    break
  fi

  echo "Deployment not ready yet, waiting 15s... (attempt $i/20)"
  sleep 15
done

if [ -z "$PREVIEW_URL" ] || [ "$PREVIEW_URL" == "null" ]; then
  echo "WARNING: Deployment not ready after 5 minutes. Skipping preview validation."
  # Continue without blocking - preview validation is enhancement, not blocker
fi
```

### Alternative: Use Branch-Based URL (Fallback)

If API polling fails, use predictable branch URL:

```bash
BRANCH=$(git branch --show-current)
# Vercel branch URL format: project-git-branch-team.vercel.app
PREVIEW_URL="body-compass-app-git-${BRANCH}-craigs-projects.vercel.app"
```

**Note:** Branch URL may show stale content if deployment is still building.

### Gather Validation Context

Before spawning the validator, extract from earlier phases:

1. **From Plan (if exists):**
   - Success criteria / acceptance criteria
   - Silver bullet (the ONE thing that proves the feature works)
   - Affected pages/routes mentioned

2. **From Diff:**
   - Changed files → derive affected components/pages
   - Modified routes or API endpoints

3. **From Test Specs:**
   - What user journeys were specified

**Save as VALIDATION_CONTEXT for the subagent prompt.**

### Run Targeted UI Validation on Preview

**Only proceed if PREVIEW_URL was obtained.**

```markdown
Task(
subagent_type: "general-purpose",
model: "sonnet",
prompt: " # Targeted Preview Deployment Validation

    **URL:** https://[PREVIEW_URL]

    ## Context From This Review

    **Plan Path:** [plan path or 'no plan']
    **Silver Bullet:** [the ONE thing that must work - from plan's success criteria]
    **Changed Files:** [list from diff]
    **Affected Pages/Components:** [derived from changed files]
    **Success Criteria:** [from plan if available]

    ## Validation Strategy

    Use Playwright browser tools:
    - `browser_navigate` - go to URLs
    - `browser_snapshot` - get page accessibility tree (preferred for verification)
    - `browser_click`, `browser_type`, `browser_fill_form` - interact with elements
    - `browser_console_messages` - check for errors

    ### 1. Smoke Test (30 sec) - ALWAYS DO
    - Navigate to app root
    - Verify app loads without crash
    - Check `browser_console_messages` for critical errors (ignore warnings)

    ### 2. Silver Bullet Validation (PRIMARY - MANDATORY)
    This is the most important check. Test the specific success criterion from the plan:
    - Navigate to the page where the feature lives
    - Execute the exact user journey that proves the feature works
    - Verify the expected outcome occurs
    - If silver bullet fails, entire validation = FAIL

    ### 3. Changed Component Validation (SECONDARY)
    For each page/component affected by the diff:
    - Navigate to page containing the changed component
    - Use `browser_snapshot` to verify it renders
    - If interactive elements were modified, test the interaction
    - Note any visual or functional regressions

    ## What NOT To Do
    - ❌ Don't test pages unrelated to the changes
    - ❌ Don't run full app regression
    - ❌ Don't test features that weren't touched
    - ❌ Don't spend time on generic 'exploration'

    ## Output Format

    PREVIEW_VALIDATION:
      url: [tested URL]

      smoke_test:
        status: PASS | FAIL
        console_errors: [critical errors only, or 'none']

      silver_bullet:
        description: [what was tested]
        status: PASS | FAIL
        details: [what happened]

      changed_components:
        - component: [name]
          page: [URL path]
          status: PASS | FAIL
          notes: [any issues]

      overall_status: PASS | FAIL | PARTIAL
      recommendation: proceed | investigate | block
      blocking_issues: [list if any]

"
)
```

### Handle Validation Results

**If PASS:** Continue to Phase 6

**If FAIL with blocking issues:**

```markdown
Preview deployment validation found issues:

- [list issues]

**Options:**

1. Fix issues and re-run review from Phase 4
2. Proceed anyway (document as known issue)
3. Investigate manually before proceeding

**Recommendation:** [based on severity]
```

Wait for Craig's decision if blocking issues found.

**If PARTIAL or minor issues:**

- Document in review report
- Continue to Phase 6

**Mark task as `completed`**

---

## Phase 6: Present Decisions to Craig

**Mark task "Present decisions" as `in_progress`**

### If No NEEDS_DECISION Items

Skip to Phase 7 (Final Report).

### If NEEDS_DECISION Items Exist

Present to Craig:

```markdown
## Review Complete: [Feature]

**Report:** `.claude/reviews/YYYY-MM-DD-HHMM-[feature].md`

### Auto-Fixed (X items)

- [brief list of what was fixed]

### Needs Your Decision (Y items)

1. **[ARCH-1]** [Title]
   - Location: `file.ts:42`
   - Issue: [description]
   - Options:
     - [A] [Option description] (Recommended)
     - [B] [Option description]
   - Trade-off: [what's at stake]

2. **[PERF-2]** [Title]
   - Location: `file.ts:89`
   - Issue: [description]
   - Options:
     - [A] [Option description]
     - [B] [Option description] (Recommended)
   - Trade-off: [what's at stake]

**How would you like to proceed?**
```

**Wait for Craig's response.**

**Mark task as `completed`**

---

## Phase 7: Handle User Decisions

**If Craig chose options:**

Spawn engineer to implement chosen options:

```markdown
Task(
subagent_type: "engineer",
model: "sonnet",
prompt: " # Implement Craig's Decisions

    Apply these changes based on Craig's choices:

    [list decisions with chosen options]

    Run tests after changes.

"
)
```

**If Craig said "skip" or "defer":**

Document as accepted risk in review report.

### Commit Additional Changes

```bash
git add -A
git commit -m "review: implement decisions for [feature]

Changes:
- [list]

Generated with [Claude Code](https://claude.ai/code)
via [Happy](https://happy.engineering)

Co-Authored-By: Claude <noreply@anthropic.com>
Co-Authored-By: Happy <yesreply@happy.engineering>"

git push
```

---

## Phase 8: Final Report

**Mark task "Final report" as `in_progress`**

```markdown
## Review Complete: [Feature]

**Status:** APPROVED
**Report:** `.claude/reviews/YYYY-MM-DD-HHMM-[feature].md`

### Summary

- **Critical:** 0 remaining (X fixed)
- **Improvements:** Y addressed, Z deferred
- **Nits:** N noted

### Changes Made

- [list key changes]

### Decisions Made

- [list decisions and outcomes]

**Ready for PR when you choose.**
```

**Mark task as `completed`**

---

## Flexibility & Overrides

### Craig Can Adjust

**"Skip security review"**
→ Don't spawn security-reviewer

**"Quick review"**
→ Spawn single comprehensive reviewer instead of 4

**"Just report, don't fix"**
→ Skip Phase 4, present all findings

**"Review these files only: [list]"**
→ Pass file list to reviewers instead of full diff

---

## Remember

**You coordinate, reviewers analyze, engineer fixes.**

Your job:

- ✅ Gather context
- ✅ Spawn reviewers in parallel
- ✅ Aggregate and categorize findings
- ✅ Coordinate auto-fixes
- ✅ Present decisions clearly
- ✅ Commit and report

NOT your job:

- ❌ Analyze code yourself
- ❌ Make architectural decisions
- ❌ Skip the auto-fix loop
- ❌ Commit without running checks

**Parallel reviewers, categorized findings, auto-fix what's clear, escalate what's not.**
