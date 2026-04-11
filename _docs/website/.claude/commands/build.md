---
description: Build new features using multi-agent research → plan → implement → verify workflow
---

**MANDATORY FIRST STEP: Create task list with TodoWrite BEFORE starting.**

You are the orchestrator coordinating specialized agents to build a new feature. **DO NOT implement code yourself—delegate to specialist agents.**

---

## Workflow Overview

1. **Create task list** (TodoWrite - high-level phases)
2. **Parallel code exploration** (3x code-explorer agents)
3. **Synthesize findings & create plan** (you)
4. **Ask Craig questions** (if ambiguities found)
5. **Present plan for approval** (WAIT for Craig)
6. **Parallel implementation** (engineer agents)
7. **Parallel quality assurance** (reviewer + tester agents)
8. **Auto-run /commit** (when all complete)

**Expected duration:** 30-60 min for medium features

---

## Phase 0: Initialize Task List

**FIRST ACTION: Use TodoWrite to create high-level task list.**

```markdown
TodoWrite:

1. Explore codebase for patterns (pending)
2. Create implementation plan (pending)
3. Get Craig's approval on plan (pending)
4. Implement core functionality (pending)
5. Run quality assurance checks (pending)
6. Commit and push changes (pending)
```

**Mark each task `in_progress` when starting, `completed` when done.**

This ensures you complete the full workflow and don't exit early.

---

## Phase 1: Parallel Code Exploration (5-10 min)

**Mark task "Explore codebase" as `in_progress`**

### Spawn 3 Code Explorer Agents Simultaneously

**CRITICAL: All 3 agents run IN PARALLEL using a single message with 3 Task calls.**

```markdown
Task(code-explorer, "Find existing patterns for [feature] in codebase.
Load CORE skill. Search features/ for similar components, hooks, and utilities.
Output to .claude/plans/research/YYYY-MM-DD-HHMM-exploration-patterns-[feature].md")

Task(code-explorer, "Identify dependencies and APIs needed for [feature].
Check package.json, search for imports of relevant libraries (e.g., Supabase, Zod).
Output to .claude/plans/research/YYYY-MM-DD-HHMM-exploration-dependencies-[feature].md")

Task(code-explorer, "Research edge cases, security patterns, and offline-first approach for [feature].
Search for validation, error handling, auth checks, and offline sync patterns.
Output to .claude/plans/research/YYYY-MM-DD-HHMM-exploration-security-[feature].md")
```

**Wait for all 3 agents to complete. Review their outputs.**

**Mark task "Explore codebase" as `completed`**

---

## Phase 2: Synthesize Findings & Create Plan (10-20 min)

**Mark task "Create implementation plan" as `in_progress`**

### Review All Exploration Reports

Read the 3 research outputs:

- `.claude/plans/research/*-patterns-*.md`
- `.claude/plans/research/*-dependencies-*.md`
- `.claude/plans/research/*-security-*.md`

### Identify Conflicts or Gaps

**Check for:**

- Multiple conflicting patterns (which to follow?)
- Missing information (need more exploration?)
- Unclear requirements (need Craig's input?)
- Architectural decisions needed

### Create Implementation Plan

**Copy template:**

```bash
cp .claude/plans/templates/build-template.md .claude/plans/YYYY-MM-DD-HHMM-[feature-name].md
```

**Fill out plan using template structure:**

1. **Context & Research** - Synthesize findings from 3 exploration reports
2. **User Journey & Requirements** - From Craig's initial request
3. **Specification** - API contracts, data model, component structure
4. **Test Strategy** - Coverage targets, test types
5. **Implementation Plan** - Phased breakdown with agent assignments
6. **Risks & Mitigations** - Potential issues identified

**Adapt template to feature complexity:**

- Simple feature (30-60 min): Sections 2, 3, 4, 5 (brief)
- Medium feature (2-4 hours): All sections, concise
- Complex feature (4+ hours): All sections, detailed

**Mark task "Create implementation plan" as `completed`**

---

## Phase 3: Questions & Approval (2-5 min)

**Mark task "Get Craig's approval on plan" as `in_progress`**

### Ask Questions If Needed

**If you found:**

- Multiple valid approaches → Use AskUserQuestion
- Unclear requirements → Use AskUserQuestion
- Architectural decisions → Use AskUserQuestion

**Format:**

```
AskUserQuestion:
Q1: Should notifications use existing push service or add new one?
  - Option A: Existing service (faster, limited features)
  - Option B: New service (more features, more setup)
Q2: Include email notifications or just in-app?
  - Option A: In-app only (simpler)
  - Option B: Email + in-app (better UX, more complex)
```

### Present Plan for Approval

**Message format:**

```markdown
I've created an implementation plan at:
`.claude/plans/YYYY-MM-DD-HHMM-[feature-name].md`

## Approach Summary

[2-3 sentence summary of proposed approach]

## Key Decisions

- **Architecture:** [Main architectural choice]
- **Dependencies:** [New libraries needed, if any]
- **Database:** [Schema changes, if any]

## Questions

[List questions from AskUserQuestion, if any]

## Proceed?

Please review the plan and let me know if the approach looks good or if you'd like any adjustments.
```

**CRITICAL: WAIT for Craig's approval. DO NOT proceed to implementation without explicit "yes" or "proceed".**

**Mark task "Get Craig's approval on plan" as `completed` after approval received**

---

## Phase 4: Parallel Implementation (15-40 min)

**Mark task "Implement core functionality" as `in_progress`**

### Analyze Implementation Phases

**Review plan's "Implementation Plan" section:**

- How many phases?
- Which phases can run in parallel?
- Which must run sequentially?

### Spawn Engineer Agents

**For parallel work, use SINGLE MESSAGE with multiple Task calls:**

```markdown
Task(engineer, "Implement Phase 2, Section A - Core Logic.
Load CORE and testing skills.
Read plan at .claude/plans/YYYY-MM-DD-HHMM-[feature].md
Read research at .claude/plans/research/_-patterns-_.md
Follow TDD: Write failing tests first, then implement.
Report when complete with verification results.")

Task(engineer, "Implement Phase 2, Section B - UI Components.
Load CORE, design-system, and testing skills.
Read plan at .claude/plans/YYYY-MM-DD-HHMM-[feature].md
Follow mobile-first patterns from design-system skill.
Follow TDD: Write failing tests first, then implement.
Report when complete with verification results.")

Task(engineer, "Implement Phase 2, Section C - Integration Tests.
Load testing skill.
Read plan at .claude/plans/YYYY-MM-DD-HHMM-[feature].md
Write integration tests for API routes and data flow.
Report when complete with test results.")
```

**For sequential work, wait for each phase to complete before spawning next.**

### Monitor Progress

**As agents report completion:**

- Verify they ran local checks (type-check, tests, lint)
- Check they followed TDD (tests first)
- Confirm coverage meets 80% minimum
- Note any issues they flagged

**If agent reports blocker:**

1. Review the issue
2. Make decision or escalate to Craig
3. Update plan if needed
4. Resume agent with guidance

**Mark task "Implement core functionality" as `completed` after all engineer agents finish**

---

## Phase 5: Quality Assurance (10-20 min)

**Mark task "Run quality assurance checks" as `in_progress`**

### Spawn QA Agents IN PARALLEL

**CRITICAL: Both agents run simultaneously using single message:**

```markdown
Task(reviewer, "Review implementation for [feature].
Load CORE, testing, and design-system skills.
Read plan at .claude/plans/YYYY-MM-DD-HHMM-[feature].md
Read research at .claude/plans/research/\*.md
Review files modified by engineer agents.
Audit for security, performance, code quality, standards compliance.
Output to .claude/plans/review/YYYY-MM-DD-HHMM-review-[feature].md
Report critical issues, important issues, and suggestions.")

Task(tester, "Validate implementation for [feature].
Load CORE, testing, and design-system skills.
Read plan at .claude/plans/YYYY-MM-DD-HHMM-[feature].md
Run test suite with coverage (pnpm test:coverage).
Verify coverage >= 80% minimum.
Perform browser testing using Chrome extension (start pnpm dev).
Test mobile responsiveness, touch interactions, offline mode.
Output to .claude/plans/testing/YYYY-MM-DD-HHMM-test-report-[feature].md
Report pass/fail status and any issues.")
```

### Review QA Results

**Read both reports:**

- `.claude/plans/review/YYYY-MM-DD-HHMM-review-[feature].md`
- `.claude/plans/testing/YYYY-MM-DD-HHMM-test-report-[feature].md`

### Handle Issues

**If critical issues found:**

1. **STOP** - Do not proceed to commit
2. Spawn engineer agent to fix critical issues
3. Re-run QA agents after fixes
4. Repeat until no critical issues

**If important issues found:**

1. Note them for addressing before next PR
2. Can proceed to commit (not blocking)
3. Add to project backlog/roadmap

**If only suggestions found:**

1. Note them for future consideration
2. Proceed to commit

**Mark task "Run quality assurance checks" as `completed` after QA passes**

---

## Phase 6: Commit Changes (5-10 min)

**Mark task "Commit and push changes" as `in_progress`**

### Auto-Run Commit Workflow

**Execute /commit command:**

The commit workflow will:

1. Check/create branch
2. Run pre-commit checks (format, lint, type-check, test, build)
3. Fix auto-fixable issues
4. Stage all changes (git add -A)
5. Commit with message
6. Push to remote

**Monitor commit process:**

- Ensure all pre-commit checks pass
- Verify commit message is clear
- Confirm push succeeds

**Mark task "Commit and push changes" as `completed` after commit successful**

---

## Phase 7: Summary & Next Steps (2 min)

### Report Completion

**Message format:**

```markdown
## Build Complete: [Feature Name]

**Feature:** [Brief description]
**Branch:** [branch-name]
**Commit:** [commit-hash]

### Deliverables

**Plan:** `.claude/plans/YYYY-MM-DD-HHMM-[feature].md`
**Research:** `.claude/plans/research/*-[feature].md` (3 files)
**Review:** `.claude/plans/review/YYYY-MM-DD-HHMM-review-[feature].md`
**Test Report:** `.claude/plans/testing/YYYY-MM-DD-HHMM-test-report-[feature].md`

### Implementation Summary

**Files created:**

- [List new files]

**Files modified:**

- [List modified files]

**Test coverage:** [X]%

### Quality Results

**Code review:**

- Critical issues: [count]
- Important issues: [count]
- Suggestions: [count]

**Testing:**

- Automated tests: [PASS/FAIL]
- Manual tests: [PASS/FAIL]
- Coverage: [X]% (target: 80%+)

### Next Steps

1. Create PR using `/pr` command
2. Address important issues before next PR: [list]
3. Consider suggestions for future: [list]

**Ready for PR creation.**
```

---

## Flexibility & Overrides

### Craig Can Override Process

**"Skip the plan, just implement"**
→ Proceed to Phase 4 directly (still run QA though)

**"Quick prototype first"**
→ Build throwaway version, then formalize if keeping

**"This is exploratory"**
→ No tests initially, validate concept first

**Trust Craig's judgment on when to follow/skip steps.**

### Mid-Workflow Adjustments

**If you discover issues while orchestrating:**

1. **STOP** current phase
2. Update plan with learnings
3. Discuss with Craig if approach changes
4. Resume with clarity

**Example:**

```
"While code exploration revealed that the existing auth pattern won't work for settings, I recommend updating the plan to use [alternative approach]. Should I proceed with this change to the plan?"
```

---

## Token Management

**Optimize for performance:**

- Run agents in parallel whenever possible (single message, multiple Tasks)
- Use Haiku for code-explorer (cheaper, faster)
- Use Sonnet for engineer, reviewer, tester (complex reasoning)
- Reuse exploration reports (don't re-explore)
- Keep plan as single source of truth (agents read it)

**Expected token usage:**

- Exploration: ~15k tokens (3 agents x 5k)
- Planning: ~5k tokens (synthesis)
- Implementation: ~30-60k tokens (3 agents x 10-20k)
- QA: ~20k tokens (2 agents x 10k)
- **Total: 70-100k tokens** for medium feature

---

## Troubleshooting

### Agent Reports Blocker

**Issue:** Agent says "Cannot proceed, need decision on X"

**Action:**

1. Review the blocker description
2. If straightforward: Make decision, resume agent
3. If architectural: Ask Craig using AskUserQuestion
4. Update plan with decision
5. Resume agent with clarity

### Tests Failing

**Issue:** Engineer reports tests won't pass

**Action:**

1. Review test output
2. Determine if bug in implementation or test
3. If implementation bug: Fix
4. If test issue: Adjust test
5. Re-run until green

### QA Finds Critical Issues

**Issue:** Reviewer or tester reports blocking bugs

**Action:**

1. **DO NOT commit**
2. Spawn engineer to fix critical issues
3. Re-run QA after fixes
4. Repeat until clean

### Craig Rejects Plan

**Issue:** Craig says "No, different approach needed"

**Action:**

1. Understand what needs changing
2. Update plan accordingly
3. Re-present for approval
4. Only proceed after approval

---

## Common Build Patterns

### Simple Feature (30-60 min)

**Example:** "Add loading spinner to camera page"

**Process:**

- Brief exploration (10 min)
- Simple plan (10 min)
- Single engineer agent (15 min)
- QA (10 min)
- Commit (5 min)

### Medium Feature (2-4 hours)

**Example:** "Add notification settings to user profile"

**Process:**

- Full exploration (15 min)
- Detailed plan (20 min)
- 2-3 engineer agents (40 min)
- Full QA (15 min)
- Commit (10 min)

### Complex Feature (4+ hours)

**Example:** "Implement food recommendation engine"

**Process:**

- Comprehensive exploration (30 min)
- Detailed plan with decision logs (40 min)
- 4-5 engineer agents in phases (120+ min)
- Full QA + manual testing (30 min)
- Commit (15 min)

---

## Remember

**You are the conductor, not the musician.**

Your job is to:

- ✅ Coordinate agents effectively
- ✅ Synthesize information
- ✅ Make tactical decisions
- ✅ Escalate strategic decisions to Craig
- ✅ Ensure quality throughout
- ✅ Complete the full workflow

Your job is NOT to:

- ❌ Write implementation code
- ❌ Run tests yourself
- ❌ Review code yourself
- ❌ Skip quality gates
- ❌ Exit early before commit

**Follow the phases. Trust the specialists. Complete the workflow.**
