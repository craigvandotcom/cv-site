---
description: Create implementation plans with validation baseline before execution
---

**MANDATORY FIRST STEP: Create task list with TodoWrite BEFORE starting.**

You are the orchestrator creating implementation plans. **DO NOT implement code—only plan.**

---

## Workflow Overview

1. **Create task list** (TodoWrite - high-level phases)
2. **Parallel code exploration** (3x code-explorer agents)
3. **Validation baseline** ("Taste the Tools" - verify you can measure success)
4. **Synthesize findings & create plan** (you)
5. **Present plan for approval** (WAIT for Craig)

**Expected duration:** 15-45 min depending on complexity

---

## Phase 0: Initialize & Classify

**FIRST ACTION: Use TodoWrite to create high-level task list.**

```markdown
TodoWrite:

1. Classify request and initialize (pending)
2. Parallel code exploration (3 agents) (pending)
3. Establish validation baseline (pending)
4. Create implementation plan (pending)
5. Get Craig's approval (pending)
6. Commit plan artifacts (pending)
```

**Mark each task `in_progress` when starting, `completed` when done.**

### Classify the Request

```
User request → Classify:
├── Type: BUILD | IMPROVE | FIX
└── Complexity: MINIMAL | MORE | A LOT (auto-detect or user-specified)
```

**Complexity Detection:**

- MINIMAL: <3 files, clear pattern, 1-2 hours work
- MORE: 3-10 files, some decisions, 2-4 hours work
- A LOT: >10 files, architectural decisions, 4+ hours work

---

## Phase 1A: Parallel Code Exploration (5-15 min)

**Mark task "Explore codebase" as `in_progress`**

### Spawn 3 Code Explorer Agents Simultaneously

**CRITICAL: All 3 agents run IN PARALLEL using a single message with 3 Task calls.**

```markdown
Task(code-explorer, model: haiku, "Find existing patterns for [feature] in codebase.
Load CORE skill. Search features/ for similar components, hooks, and utilities.
Output to .claude/plans/research/YYYY-MM-DD-HHMM-exploration-patterns-[feature].md")

Task(code-explorer, model: haiku, "Identify dependencies and APIs needed for [feature].
Check package.json, search for imports of relevant libraries (e.g., Supabase, Zod, React Query).
Output to .claude/plans/research/YYYY-MM-DD-HHMM-exploration-dependencies-[feature].md")

Task(code-explorer, model: haiku, "Research constraints for [feature].
Search for: validation, error handling, auth checks, offline sync, mobile-first patterns.
Output to .claude/plans/research/YYYY-MM-DD-HHMM-exploration-constraints-[feature].md")
```

**Wait for all 3 agents to complete. Review their outputs.**

**Mark task "Explore codebase" as `completed`**

---

## Phase 1B: Validation Baseline ("Taste the Tools") (5-10 min)

**Mark task "Establish validation baseline" as `in_progress`**

**Purpose:** Capture current state AND verify you can actually measure success BEFORE planning.

### Step 1: Identify Validation Method

Based on task type, determine how success will be validated:

| Task Type   | Primary Validation       | Secondary             |
| ----------- | ------------------------ | --------------------- |
| UI Feature  | Playwright journey       | Screenshot comparison |
| API Change  | Response shape assertion | Integration tests     |
| Bug Fix     | Reproduction script      | Unit tests            |
| Performance | Baseline metrics         | Load tests            |

### Step 2: Capture Current State

**For UI features (ALWAYS capture both):**

```markdown
1. Start dev server: pnpm dev (if not running)
2. Navigate to relevant page using Playwright MCP
3. Take accessibility snapshot: mcp**playwright**browser_snapshot
   → Save as: research/YYYY-MM-DD-HHMM-baseline-snapshot-[feature].md
4. Take screenshot: mcp**playwright**browser_take_screenshot
   → Save as: research/YYYY-MM-DD-HHMM-baseline-screenshot-[feature].png
5. Document current state

These become the "BEFORE" for automatic before/after comparison at validation.
```

**For API changes:**

```markdown
1. Hit current endpoint (if exists)
2. Record response shape
3. Note any errors or limitations
```

**For bug fixes:**

```markdown
1. Follow reproduction steps
2. Document the broken behavior (screenshot/snapshot)
3. Confirm you can see the bug
```

**Save to:** `.claude/plans/research/YYYY-MM-DD-HHMM-baseline-[feature].md`

### Step 3: "Taste the Tools" (Verify Validation Capability)

**Run these checks to confirm you can validate success:**

```markdown
## Tool Verification Checklist

### Unit/Integration Tests

- [ ] Run: pnpm test
- [ ] Result: [X passing / Y failing]
- [ ] Status: ✅ Can run tests | ❌ BLOCKED

### Browser Access (Playwright MCP)

- [ ] Navigate to: /[relevant-page]
- [ ] Result: [Can access | Cannot access]
- [ ] Status: ✅ Can browse | ❌ BLOCKED

### Dev Server

- [ ] Check: pnpm dev running on localhost:3000
- [ ] Result: [Running | Not running]
- [ ] Status: ✅ Accessible | ❌ BLOCKED

### API Endpoints (if applicable)

- [ ] Endpoint: /api/[endpoint]
- [ ] Result: [Response code]
- [ ] Status: ✅ Reachable | ❌ BLOCKED
```

**CRITICAL: If ANY tool is BLOCKED, assess and handle:**

**IF all tools blocked:**

```markdown
⚠️ **Validation Blocker Detected**

Cannot proceed with planning because:

- [Tool X] is not accessible
- [Reason/error]

**Recovery procedure:**

1. Start dev server: `pnpm dev` (if applicable)
2. Check MCP connection: verify Playwright MCP server running
3. Retry validation after fixes
4. If still blocked: escalate to Craig with error details

**Recommended action:** [What needs to be fixed]
```

STOP and await Craig's confirmation that tools are ready before proceeding.

**IF partial tools available (some work, some blocked):**

```markdown
⚠️ **Partial Validation Available**

**Working tools:**

- ✅ [Tool A] - available
- ✅ [Tool B] - available

**Blocked tools:**

- ❌ [Tool C] - [reason]

**Proposed alternative validation:**
Instead of [blocked approach], we will validate using [available tools].

**Adjusted success criterion:**
[How we'll measure success with available tools]
```

Get Craig's approval before proceeding with adjusted criteria.

### Step 4: Define Success Criterion (Silver Bullet)

**Based on task type, define the measurable "Silver Bullet":**

```markdown
## Success Criterion (Silver Bullet)

**Type:** [Journey | Screenshot | Test | Output | Metric]

**Definition:**
[What exactly constitutes success - machine-verifiable]

**Validation Command:**
[Exact command to run - e.g., pnpm test tests/e2e/camera-capture.spec.ts]

**Expected Outcome:**
[What passing looks like - e.g., "All 3 assertions pass, journey completes"]
```

**Example for UI feature:**

```markdown
Type: Journey
Definition: User can navigate to /meals, click camera button, capture ingredient, see it added to meal log
Validation Command: pnpm test:e2e tests/e2e/camera-capture.spec.ts
Expected Outcome: Journey completes, ingredient visible in meal list
```

### Step 5: Define Test Specifications (Machine-Readable)

**CRITICAL: Tests are designed here, built in /work phase.**

Define what tests need to exist. These specs are "hardcoded" in the plan—engineer cannot modify them during implementation.

**Use this exact YAML format for /work parsing:**

```yaml
## Test Specifications

test_specs:
  silver_bullet:
    file: 'tests/e2e/[feature].spec.ts'
    type: 'Journey' # Journey | Screenshot | API | Performance | Custom
    description: '[What this test verifies]'
    assertions:
      - '[First assertion - e.g., Camera button visible and clickable]'
      - '[Second assertion - e.g., Capture triggers ingredient detection]'
      - '[Third assertion - e.g., Ingredient appears in meal log]'

  supporting_tests:
    - name: '[Test 1 Name]'
      file: 'tests/unit/[path].test.ts'
      type: 'Unit'
      description: '[What it verifies]'
      cases:
        - '[happy path]'
        - '[edge case]'
        - '[error case]'

    - name: '[Test 2 Name]'
      file: 'tests/integration/[path].test.ts'
      type: 'Integration'
      description: '[What it verifies]'
      cases:
        - '[case 1]'
        - '[case 2]'

    - name: '[Test 3 Name]'
      file: 'tests/unit/[path].test.ts'
      type: 'Unit'
      description: '[What it verifies]'
      cases:
        - '[case 1]'
        - '[case 2]'
```

**Why structured YAML format?**

- Machine-parseable by /work (Engineer #1 reads specs reliably)
- Tests designed before implementation thinking → no "cheating"
- Craig reviews and approves test specs
- Engineer implements to spec, can't modify requirements
- Audit trail of what was expected

### Step 6: Document Baseline vs Target

```markdown
## Baseline vs Target

| Aspect         | Current State      | Target State        |
| -------------- | ------------------ | ------------------- |
| [Feature area] | [What exists now]  | [What should exist] |
| [Behavior]     | [Current behavior] | [Desired behavior]  |
| [Test status]  | [Current coverage] | [Expected coverage] |
```

**Mark task "Establish validation baseline" as `completed`**

---

## Phase 2: Synthesize Findings & Create Plan (10-30 min)

**Mark task "Create implementation plan" as `in_progress`**

### Review All Research

Read the outputs:

- `.claude/plans/research/*-patterns-*.md`
- `.claude/plans/research/*-dependencies-*.md`
- `.claude/plans/research/*-constraints-*.md`
- `.claude/plans/research/*-baseline-*.md`

### Check for Conflicts/Gaps

**Ask yourself:**

- Multiple conflicting patterns found?
- Missing information needing more exploration?
- Unclear requirements needing Craig's input?
- Architectural decisions required?

### Create Plan Document

**Create plan file:**

```
.claude/plans/YYYY-MM-DD-HHMM-[feature-name].md
```

### Select Template Based on Complexity

| Complexity  | Criteria                              | Template Sections                                                            |
| ----------- | ------------------------------------- | ---------------------------------------------------------------------------- |
| **MINIMAL** | <3 files, clear pattern, 1-2 hours    | Summary, Success Criterion, Implementation, Validation                       |
| **MORE**    | 3-10 files, some decisions, 2-4 hours | All 7 sections (Context, Outcome, Journey, Spec, Success, Phases, Risks)     |
| **A LOT**   | >10 files, architectural, 4+ hours    | All MORE sections + Decision Log, Alternatives, Phased Rollout, Dependencies |

**Use template structure (adapt to complexity):**

#### For MINIMAL complexity:

```markdown
# [Feature Name]

## Summary

[1-2 sentences]

## Success Criterion

[From Phase 1B]

## Implementation

1. [Step 1]
2. [Step 2]
3. [Step 3]

## Validation

[How to verify success]
```

#### For MORE complexity:

```markdown
# [Feature Name]

## Context & Research

[Synthesized from 3 exploration reports]

## Outcome Definition

[What success looks like - from Phase 1B baseline]

## User Journey (if UI)

[Flow description]

## Technical Specification

- API contracts
- Data model changes
- Component structure

## Success Criteria

[From Phase 1B - measurable!]

## Implementation Phases

### Phase 1: [Foundation]

### Phase 2: [Core Logic]

### Phase 3: [UI/Integration]

### Phase 4: [Testing]

## Risks & Mitigations

[What could go wrong]
```

#### For A LOT complexity:

All sections from MORE, plus:

- Decision log (alternatives considered)
- Phased rollout plan
- Detailed test matrix
- Dependencies/blockers

**Mark task "Create implementation plan" as `completed`**

---

## Phase 3: Present for Approval (2-10 min)

**Mark task "Get Craig's approval" as `in_progress`**

### Ask Questions If Needed

**If ambiguities found, use AskUserQuestion:**

```
AskUserQuestion:
Q1: [Decision needed]
  - Option A: [Description] (Recommended)
  - Option B: [Description]
```

### Present Plan Summary

```markdown
## Plan Created: [Feature Name]

**Plan:** `.claude/plans/YYYY-MM-DD-HHMM-[feature].md`
**Research:** `.claude/plans/research/` (4 files)

### Summary

[2-3 sentences describing approach]

### Type & Complexity

- **Type:** BUILD | IMPROVE | FIX
- **Complexity:** MINIMAL | MORE | A LOT

### Validation Baseline

- **Current state captured:** ✅
- **Tools verified:** ✅ (tests run, browser accessible, APIs reachable)
- **Success criterion:** [Brief description]

### Key Decisions

- **Architecture:** [Main choice]
- **Dependencies:** [New libraries, if any]
- **Database:** [Schema changes, if any]

### Questions

[If any, from AskUserQuestion]

---

**Proceed with implementation?**
```

**CRITICAL: WAIT for Craig's explicit approval.**

- "yes" or "proceed" → Plan approved, ready for /build
- "adjust [X]" → Update plan, re-present
- "no" → Discuss concerns, revise approach

**Mark task "Get Craig's approval" as `completed` after approval received**

---

## Phase 4: Finalize & Hand Off

### Update Plan Status

Add to plan document:

```markdown
---
**Status:** Approved - Ready for Implementation
**Approved:** YYYY-MM-DD
---
```

### Safety Check (Before Commit)

```bash
git status --short
```

**Review output:**

- **If ANY deletions (D):** STOP and ask Craig "You're about to delete X files. Is this intentional?"
- Wait for confirmation before proceeding if deletions present

### Commit Plan Artifacts (Non-Negotiable)

**Commit all planning work before ending the session:**

```bash
git add .claude/plans/research/*.md
git add .claude/plans/YYYY-MM-DD-HHMM-[feature].md
git commit -m "$(cat <<'EOF'
docs(plan): [feature-name] - approved implementation plan

Research: patterns, dependencies, constraints
Baseline: current state captured, tools verified
Success criterion: [brief description]
Tests designed: 1 Silver Bullet + [X] supporting tests

Generated with [Claude Code](https://claude.ai/code)
via [Happy](https://happy.engineering)

Co-Authored-By: Claude <noreply@anthropic.com>
Co-Authored-By: Happy <yesreply@happy.engineering>
EOF
)"
git push
```

**Why commit plans?**

- Low risk (no code changes)
- Preserves research investment
- Enables async implementation (another session can pick up)
- Follows "Land the Plane" principle: work doesn't exist until pushed

### Report Completion & Hand-Off

```markdown
## Plan Complete: [Feature Name]

**Plan:** `.claude/plans/YYYY-MM-DD-HHMM-[feature].md`
**Status:** Approved & Committed

### Ready for Implementation

**Start command:**

> /work

**Key context:**

- Plan: `.claude/plans/YYYY-MM-DD-HHMM-[feature].md`
- Success criterion: [from plan]
- Watch out for: [one key constraint or pattern discovered]

**Plan committed. Ready for implementation.**
```

---

## Flexibility & Overrides

### Craig Can Adjust Process

**"Just do a quick plan"**
→ Skip parallel exploration, use single-pass analysis

**"Focus on [specific aspect]"**
→ Emphasize that area in plan

**"Skip validation baseline"**
→ Proceed without tool verification (risky but fast)

**Trust Craig's judgment on when to follow/skip steps.**

---

## Integration with /work

When `/work` is run after `/plan`:

1. `/work` discovers the most recent approved plan
2. Reads test specifications from plan
3. Spawns Engineer #1: Build tests from specs, verify RED
4. Main Agent reviews test quality
5. Spawns Engineer #2: Implement until all tests GREEN
6. Commit + push on success
7. Hand off to `/review`

**Key principle:** Tests are designed in /plan (hardcoded specs), built and run in /work. Engineer cannot modify the specifications—only implement to satisfy them.

**Two engineers, same role:** Context isolation between test creation and implementation prevents "cheating". Both use `engineer` agent with different prompts.

See: `.claude/skills/CORE/references/tdd-2.0.md` for framework details.

---

## Token Management

**Optimize for efficiency:**

- Use Haiku for code-explorer agents (cheaper, faster)
- Run explorers in parallel (single message, 3 Task calls)
- Keep baseline document concise
- Reuse exploration reports (don't re-explore)

**Expected token usage:**

- Exploration: ~15k tokens (3 agents x 5k)
- Baseline: ~5k tokens
- Planning: ~5k tokens
- **Total: 25-35k tokens** for typical plan

---

## Remember

**Planning is thinking, not doing.**

Your job is to:

- ✅ Coordinate exploration agents
- ✅ Verify validation capability
- ✅ Capture current state
- ✅ Define measurable success
- ✅ Synthesize into actionable plan
- ✅ Get Craig's approval

Your job is NOT to:

- ❌ Write implementation code
- ❌ Run tests beyond verification
- ❌ Actually implement features
- ❌ Proceed without approval

**Follow the phases. Establish the baseline. Define success. Get approval.**
