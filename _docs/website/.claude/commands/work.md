---
description: Execute approved plans - build tests from specs, implement until GREEN, commit and push
---

**MANDATORY FIRST STEP: Create task list with TodoWrite BEFORE starting.**

You are the orchestrator executing an approved plan. Coordinate engineers to build tests and implement features.

---

## Workflow Overview

1. **Create task list** (TodoWrite)
2. **Load approved plan** (discover and validate)
3. **Spawn Engineer #1** (build tests from specs, verify RED)
4. **Review tests** (quality check)
5. **Spawn Engineer #2** (implement until GREEN)
6. **Commit + push** (on success)
7. **Hand-off to /review**

**Expected duration:** 30-90 min depending on complexity

**Ralph-compatible:** This workflow works as single run OR with `/ralph-loop` for iteration.

---

## Checkpoint System

**Purpose:** Enable mid-workflow recovery without restarting from scratch.

### How Checkpoints Work

After each major phase, save progress to `.claude/plans/checkpoints/[plan-name]-checkpoint.md`:

```markdown
# Checkpoint: [feature-name]

**Plan:** [path to plan file]
**Phase:** [current phase number]
**Status:** [in_progress | paused | blocked]
**Last updated:** [timestamp]

## Completed

- [ ] Phase 0: Initialize - [completed/pending]
- [ ] Phase 1: Load plan - [completed/pending]
- [ ] Phase 2: Engineer #1 tests - [completed/pending]
- [ ] Phase 3: Review tests - [completed/pending]
- [ ] Phase 4: Engineer #2 implement - [completed/pending]
- [ ] Phase 5: Quality gate - [completed/pending]
- [ ] Phase 6: Commit - [completed/pending]

## State Data

TEST_FILES: [list of test files created]
IMPLEMENTATION_FILES: [list of files modified]
LAST_TEST_OUTPUT: [brief summary]

## Resume Instructions

[What to do to continue from current checkpoint]
```

### Resuming from Checkpoint

**On `/work` start, check for existing checkpoint:**

```markdown
1. Check .claude/plans/checkpoints/ for [plan-name]-checkpoint.md
2. If exists and Status != completed:
   - Ask: "Found checkpoint at Phase [X]. Resume or restart?"
   - If resume: Skip completed phases, continue from checkpoint
   - If restart: Delete checkpoint, start fresh
3. If no checkpoint: Start from Phase 0
```

### When to Update Checkpoint

- After Phase 2 (tests created)
- After Phase 4 (implementation complete)
- If blocked or paused
- Before any long-running operation

---

## Phase 0: Initialize Task List

**FIRST ACTION: Use TodoWrite to create task list.**

```markdown
TodoWrite:

1. Load and validate approved plan (pending)
2. Spawn Engineer #1 to build tests (pending)
3. Review test quality (pending)
4. Spawn Engineer #2 to implement (pending)
5. Quality gate - verify success (pending)
6. Run pre-commit checks and commit (pending)
7. Hand-off to /review (pending)
```

---

## Phase 1: Load Plan (2 min)

**Mark task "Load and validate approved plan" as `in_progress`**

### Branch Safety Check (CRITICAL)

```bash
# Check current branch
CURRENT_BRANCH=$(git branch --show-current)
echo "Current branch: $CURRENT_BRANCH"
```

**If on main or master:** STOP. You must create a feature branch before any commits.

```bash
# If on main, create a feature branch based on the plan name
git checkout -b feat/[plan-name]
# Example: git checkout -b feat/camera-capture
```

**NEVER commit directly to main.** All commits go to feature branches, then merge via PR.

### Discover Approved Plan

```markdown
1. Search .claude/plans/ for files with "Status: Approved" OR "Approved - Ready for Implementation"
2. If multiple, use most recent by date prefix (YYYY-MM-DD-HHMM)
3. If none, STOP: "No approved plan found. Run /plan first."
```

### Extract From Plan

Read and extract:

```markdown
PLAN_PATH: [path to plan file]

SILVER_BULLET:
type: [Journey | Screenshot | Test | Output | Metric]
definition: [from plan]
validation_command: [from plan]
expected_outcome: [from plan]

TEST_SPECS:
silver_bullet_test:
file: [path from plan]
type: [from plan]
description: [from plan]
assertions: [list from plan]

supporting_tests: - file: [path]
type: [type]
description: [what it tests]
cases: [list] - [more tests...]
```

**Mark task as `completed`**

---

## Phase 2: Engineer #1 - Build Tests (10-20 min)

**Mark task "Spawn Engineer #1 to build tests" as `in_progress`**

### Spawn Engineer to Build Tests

```markdown
Task(
subagent_type: "engineer",
model: "sonnet",
prompt: " # Engineer Task: Build Tests from Specifications

    Read the approved plan at: [PLAN_PATH]

    ## Your Task

    Build the test files exactly as specified in the plan's Test Specifications section.

    ### Test Specs from Plan

    **Silver Bullet Test:**
    - File: [path from plan]
    - Type: [type]
    - Assertions: [list from plan]

    **Supporting Tests:**
    [list from plan]

    ### Requirements

    1. Create each test file at the specified path
    2. Implement all assertions listed in specs
    3. Follow existing test patterns in codebase
    4. Run each test and verify it FAILS (RED state)

    ### Output

    Report:
    - Files created
    - Run command for each test
    - Failure output confirming RED state

    **Do NOT implement the feature. Only write tests.**

"
)
```

### Wait for Engineer #1

Engineer returns:

- Test file paths created
- Run commands
- RED state confirmation (all tests failing)

**Mark task as `completed`**

---

## Phase 3: Review Tests (5 min)

**Mark task "Review test quality" as `in_progress`**

### Verify Test Quality

**Check:**

1. **Tests match plan specs**
   - All specified assertions present?
   - File paths match plan?

2. **Tests are RED**
   - Confirmed failing output?
   - Tests fail for right reason (missing implementation, not errors)?

3. **Tests are meaningful**
   - Would passing tests prove the feature works?
   - Edge cases covered per spec?

### If Issues Found

Provide feedback to Engineer #1:

```markdown
"Adjust tests:

- Issue: [specific problem]
- Fix: [what to change]
- Re-verify RED state after changes."
```

### If Satisfied

Proceed to implementation.

**Mark task as `completed`**

---

## Phase 4: Engineer #2 - Implement (15-60 min)

**Mark task "Spawn Engineer #2 to implement" as `in_progress`**

### Spawn Engineer to Implement

````markdown
Task(
subagent_type: "engineer",
model: "sonnet",
prompt: " # Engineer Task: Implement Feature

    Read the approved plan at: [PLAN_PATH]

    ## Test Locations

    - Silver Bullet: [path from Engineer #1]
    - Supporting: [paths from Engineer #1]

    ## Run Command

    [combined test command - e.g., pnpm test]

    ## Your Task

    Implement the feature to make all tests pass.

    ### Constraints

    - **Do NOT modify test files** - tests are fixed specifications
    - Follow existing codebase patterns
    - Run tests after each significant change
    - Handle errors gracefully

    ### Test Modification Self-Check (MANDATORY)

    **Before reporting completion, you MUST verify:**

    ```bash
    git diff --name-only [test file paths]
    ```

    **If output shows ANY test file changes:**
    1. STOP - you have violated the specification
    2. Run: `git checkout [test file paths]` to restore originals
    3. Re-implement without modifying tests
    4. Report the violation in your completion summary

    **Why:** Tests are hardcoded specifications from /plan. Modifying them defeats TDD.

    ### Loop

    1. Run all tests
    2. If failing:
       - Read error output
       - Diagnose root cause
       - Fix implementation
       - Run tests again
    3. Repeat until ALL GREEN

    ### When Complete

    **MANDATORY completion report format:**

    ```markdown
    ## Engineer Completion Report

    ### Status: [SUCCESS | PARTIAL | BLOCKED]

    ### Test Results
    - Command: `[test command run]`
    - Output: [PASS/FAIL summary]
    - All tests passing: [YES/NO]

    ### Test Modification Check
    - Ran: `git diff --name-only [test files]`
    - Test files modified: [YES/NO]
    - If YES: [explanation and remediation taken]

    ### Files Created
    - [list with line counts]

    ### Files Modified
    - [list with change summary]

    ### Implementation Summary
    [2-3 sentences describing approach taken]

    ### Issues Encountered
    [List any problems and how resolved, or 'None']
    ```

    If using Ralph loop, output: <promise>ALL_TESTS_GREEN</promise>

"
)
````

### Monitor Progress

Engineer implements iteratively until all tests pass.

**If using Ralph loop:** This phase runs via `/ralph-loop` with the engineer prompt above.

**Mark task as `completed`** when Engineer reports ALL GREEN

---

## Phase 5: Quality Gate - Verify Success (5 min)

**Mark task "Verify all tests GREEN" as `in_progress`**

### Independent Verification

**Don't trust engineer's interpretation of "success"—verify yourself.**

1. **Run the Silver Bullet test directly:**

```bash
[validation_command from plan]
```

2. **Run full test suite:**

```bash
pnpm test
```

3. **Check engineer didn't modify test files:**

```bash
git diff --name-only [test file paths]
# Should show no changes to test files
```

### Verify Against Plan's Success Criterion

**Read the plan's Success Criterion section again:**

- Does current state match the defined "Expected Outcome"?
- Did engineer interpret success correctly?
- Any edge cases missed?

### If Verification Fails

| Issue                     | Action                                     |
| ------------------------- | ------------------------------------------ |
| Tests actually failing    | Restart Engineer #2 with error details     |
| Engineer modified tests   | `git checkout` tests, restart with warning |
| Success interpreted wrong | Clarify requirements, restart              |
| Edge cases missed         | Add to prompt, restart                     |

### If Verification Passes

**Mark task as `completed`**

---

## Phase 6: Commit Flow (5 min)

**Mark task "Commit and push" as `in_progress`**

### Step 1: Branch Check

```bash
git branch --show-current
```

**If on main:**

- Create new branch: `git checkout -b feat/[feature-name]`

**If on feature branch:**

- Continue

### Step 2: Safety Check (Before Changes)

```bash
git status --short
```

**Review output:**

- **If ANY deletions (D):** STOP and ask Craig "You're about to delete X files. Is this intentional?"
- Wait for confirmation before proceeding if deletions present
- Ensure all expected files are present

### Step 3: Pre-Commit Checks

**Run ALL checks in order:**

```bash
# 1. Format
pnpm format

# 2. Lint
pnpm lint

# 3. Type check
pnpm type-check

# 4. Tests
pnpm test

# 5. Build check
pnpm build:check
```

**If any check fails:**

- Fix issues automatically where possible
- Report remaining issues
- Do NOT commit until all checks pass

**Escalation rules for pre-commit failures:**

| Check               | Auto-fixable            | Escalate to Craig           |
| ------------------- | ----------------------- | --------------------------- |
| Format              | YES - run `pnpm format` | Never                       |
| Lint (unused vars)  | YES - remove            | Never                       |
| Lint (import order) | YES - reorder           | Never                       |
| Lint (logic errors) | NO                      | If fix changes behavior     |
| Type errors         | Sometimes               | If requires API change      |
| Test failures       | NO                      | Always - don't modify tests |
| Build errors        | Sometimes               | If architecture issue       |

**If escalation needed:**

```markdown
Pre-commit check failed. Escalating to Craig.

**Check:** [which check]
**Error:** [error message]
**Why escalating:** [why can't auto-fix]
**Options:**

1. [Option A]
2. [Option B]
```

### Step 4: Stage All Changes

```bash
# Stage ALL changes (never selective)
git add -A
```

**CRITICAL:** The `pnpm format` command may modify files - these MUST be included. Never use selective staging.

### Step 5: Verify Staging

```bash
git status --short
```

**Check:**

- All files should show as staged (M/A, not " M")
- Double-check for unexpected deletions
- If unstaged files appear, run `git add -A` again

### Step 6: Commit and Push

```bash
git commit -m "feat([feature]): [description]

- Implemented [feature summary]
- Tests: [X] passing
- Silver Bullet: [type] verified

Generated with [Claude Code](https://claude.ai/code)
via [Happy](https://happy.engineering)

Co-Authored-By: Claude <noreply@anthropic.com>
Co-Authored-By: Happy <yesreply@happy.engineering>"

git push -u origin [branch-name]
```

**Mark task as `completed`**

---

## Phase 7: Hand-Off

### Report Completion

```markdown
## Work Complete: [Feature Name]

**Plan:** [plan file path]
**Silver Bullet:** [type and definition]

### Implementation Summary

**Files created:**

- [list]

**Files modified:**

- [list]

### Test Results

- Silver Bullet: PASS
- Supporting tests: X/X passing
- Full test suite: No regressions

### Committed

- Commit: [hash]
- Pushed to: [branch]

### Ready for Review

Run `/review` to proceed with code review and QA.
```

---

## Using with Ralph Loop

This workflow is compatible with `/ralph-loop` for automated iteration.

### Single Run (Default)

```bash
/work
```

Main Agent orchestrates both engineers sequentially.

### Ralph Loop (Automated Iteration)

For the implementation phase only:

```bash
/ralph-loop "
[Engineer #2 prompt from Phase 4]
" --max-iterations 15 --completion-promise "ALL_TESTS_GREEN"
```

**Note:** Test building (Engineer #1) should run before Ralph loop starts. Ralph is for implementation iteration only.

### Ralph Loop Timeout Handling

**If max iterations reached without `ALL_TESTS_GREEN`:**

```markdown
## Ralph Loop Timeout

**Iterations:** [X]/[max]
**Completion promise:** NOT fulfilled
**Last test output:**
[summary of final test state]

**Analysis:**
[Why tests aren't passing - pattern in failures]

**Options:**

1. Increase iterations: /ralph-loop ... --max-iterations [higher]
2. Simplify approach: [what could be simplified]
3. Escalate to Craig: [specific blocker description]

**Recommendation:** [which option and why]
```

**Save checkpoint before escalating so work isn't lost.**

---

## Flexibility & Overrides

### Craig Can Adjust

**"Skip test building, use existing tests"**
→ Skip Phase 2, point Engineer #2 at existing tests

**"I'll write the tests"**
→ Skip Phase 2, Craig provides test files

**"Just implement, I'll verify"**
→ Skip Phase 5 verification, go straight to commit

**"More iterations"**
→ Increase `--max-iterations` in Ralph loop

---

## Troubleshooting

### Engineer #1 Creates Wrong Tests

**Issue:** Tests don't match plan specs

**Action:**

1. Reject in review phase
2. Provide specific feedback
3. Re-run Engineer #1

### Engineer #2 Stuck

**Issue:** Same error for many iterations

**Action:**

1. Review what was tried
2. Check if test spec is achievable
3. Provide guidance or escalate to Craig

### Engineer #2 Modified Tests

**Issue:** Test files changed during implementation

**Action:**

1. STOP immediately
2. `git checkout` test files
3. Restart Engineer #2 with warning
4. Report violation

### Tests Pass But Feature Broken

**Issue:** Tests insufficient

**Action:**

1. Test specs in plan were incomplete
2. Add more tests to specs
3. Re-run from Phase 2

---

## Remember

**You coordinate, engineers execute.**

Your job:

- ✅ Load and validate plans
- ✅ Spawn engineers with clear prompts
- ✅ Review test quality
- ✅ Verify success independently
- ✅ Commit and push

NOT your job:

- ❌ Write tests yourself
- ❌ Implement features yourself
- ❌ Skip verification
- ❌ Trust without verifying

**Two engineers, one role, isolated context. Tests from specs, implementation to pass.**
