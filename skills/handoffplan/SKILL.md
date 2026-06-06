---
name: handoffplan
description: Run /handoff to capture session data, then write a phased implementation plan that references it. Creates beads for tracking.
user_invocable: true
triggers:
  - handoffplan
  - handoff with a plan
  - make this into a plan
  - create a plan and handoff
  - plan this out
argument-hint: [optional context about what to plan]
---

# Handoff Plan

**IMPORTANT: This skill writes files. You MUST NOT be in Claude Code's built-in plan mode.**
If you are currently in plan mode, **exit plan mode first** (use ExitPlanMode) before proceeding.

**Step 1: Run `/handoff` to create the data file — FULL TWO-PHASE PROCESS.**

Execute the full `/handoff` skill first, including the **mandatory two-phase write process**:
- **Phase 1:** Write the handoff file (all sections, narrative + evidence)
- **Phase 2 (MANDATORY for Deep and Chunked passes):** Read it back, scan conversation for uncaptured data, use Edit to expand toward the ceiling

**Do NOT skip or abbreviate the handoff. Do NOT skip Phase 2.**
**Do NOT ask to close the session after the handoff** — skip Step 8 of the handoff skill.
**Do NOT enter Claude Code plan mode at any point.**

**Arguments:** $ARGUMENTS

---

**CHECKPOINT before Step 2:** Count the handoff's lines. If under its pass minimum (Quick: 150/250, Deep: 300, Chunked: 500), STOP and run the handoff's Phase 2 gap research pass now.

---

**Step 2: Write the plan file.**

Same directory as the handoff, mirrored naming:
- Handoff: `HANDOFF_{chain_tag}_{slug}_{date}.md`
- Plan: `PLAN_{chain_tag}_{slug}_{date}.md`

### What makes a good plan

- **Grounded in the handoff data.** Every phase traces back to evidence.
- **Specific enough to execute without the conversation.** No vague "investigate X".
- **Honest about unknowns.** Include how to handle both outcomes.
- **Referencing, not duplicating.** Data lives in the handoff. The plan points to it.

### Line budget: 120-250 lines

### Plan Structure

```markdown
# {One-line summary of what we're planning}

**Date:** {YYYY-MM-DD}
**Status:** PLANNED
**Bead(s):** {active bead IDs, or "none"}
**Epic:** {parent epic/initiative name, if any}
**Chain:** `{chain_tag}` seq `{N}` (copied from paired handoff)
**Context:** See `{handoff_file_name}` for session data, test results, and prior approaches.

---

## Problem Statement
{3-5 sentences with key numbers. Reference handoff for full data.}

## Key Findings
{5-8 bullets — conclusions, not raw data. Each → drives Phase N.}

## Anti-Goals (What NOT To Do)
{2-5 bullets of rejected approaches from "What We Tried".}

## Plan

### Phase 1: {name}
**Goal:** {One sentence}
**Why this approach:** {1-2 sentences connecting to evidence}
{6-10 bullet implementation steps with HOW, not just WHAT}
**Files:** {files to modify/create}
**Validates with:** {test commands + success criteria with numbers}
**Rollback:** {what to revert if this phase fails}

### Phase 2: {name}
{Same format}

## Dependencies & Order
{2-5 bullets on phase ordering and parallelism.}

## Risks & Mitigations
{3-6 bullets: risk + likelihood + mitigation}

## Success Criteria
{3-6 measurable outcomes referencing baseline numbers from handoff.}

## Quick Start
\`\`\`bash
cat {handoff_file_path}
{3-5 key files to read}
{baseline verification command}
{first concrete action}
\`\`\`
```

---

**Step 3: Create beads for phases (if available).**

```bash
bd create --title="Phase 1: {name}" --description="{what and why}" --type=task --priority=2
bd create --title="Phase 2: {name}" --description="{what and why}" --type=task --priority=2
bd dep add {phase2_id} {phase1_id}
```

**Step 4: Persist to memory (if available).**

```bash
bd remember "Plan written to {plan_path}, handoff to {handoff_path}. Next: Phase 1 — {first action}"
```

**Step 5: Report.**

- Both files written and their paths
- Line counts for each
- The "First Action" from Quick Start
- Beads created for each phase (if available)

**Step 6: Commit and close.**

1. Commit session work:
   ```
   session: {slug} [{chain_tag}]

   {One-line summary of what this session accomplished}

   Handoff: {handoff_filename}
   Plan: {plan_filename}
   Bead(s): {bead_ids or "none"}

   Generated with [Claude Code](https://claude.ai/code)

   Co-Authored-By: Claude <noreply@anthropic.com>
   ```

2. Output the paste prompt — **match the user's conversation language.** Output the 繁體中文 version if the user communicates mainly in Traditional Chinese, otherwise the English version. Output ONLY one version.

   **English version:**
   ```
   -------------------------------------------------------
   PASTE THIS INTO YOUR NEXT SESSION:
   -------------------------------------------------------
   Read `{plan_file_path}` and `{handoff_file_path}` (seq {N}, {chain_tag}).
   Execute the plan starting at Phase 1. Beads are already created with dependencies.

   Your first actions:
   1. Read the plan file
   2. Claim Phase 1: `bd update {phase1_bead} --claim`
   3. Read the source files listed in the plan's Quick Start
   4. Start coding Phase 1's first concrete action: {first_action}

   Do NOT onboard, explore, or ask questions. The plan has everything. Build.
   -------------------------------------------------------
   ```

   **繁體中文版：**
   ```
   -------------------------------------------------------
   貼到你的下一個 SESSION：
   -------------------------------------------------------
   讀取 `{plan_file_path}` 和 `{handoff_file_path}`（seq {N}，{chain_tag}）。
   從 Phase 1 開始執行計畫。Beads 已建立並設好相依關係。

   你的第一批動作：
   1. 讀取計畫檔案
   2. 認領 Phase 1：`bd update {phase1_bead} --claim`
   3. 讀取計畫 Quick Start 裡列出的原始檔案
   4. 開始撰寫 Phase 1 的第一個具體動作：{first_action}

   不要 onboard、探索或提問。計畫裡有所有資訊。直接開始建構。
   -------------------------------------------------------
   ```

---

## Rules

1. **Handoff first, always.** Run the full `/handoff` skill. Don't abbreviate it.
2. **Plan references handoff, never duplicates.** Data lives in the handoff.
3. **Every phase traces to evidence.** "Why this approach" must connect to findings.
4. **Anti-Goals prevent re-work.** Explicitly state what NOT to do.
5. **Phases explain HOW, not just WHAT.**
6. **Success criteria use baseline numbers.**
7. **Rollback per phase.**
8. **Same naming for paired files.**
9. **Always close the session.**
10. **The paste prompt is an execution prompt, not an onboarding prompt.**
