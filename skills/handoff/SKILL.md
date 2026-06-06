---
name: handoff
description: Create a structured session handoff when context is running low or work is pausing. Deep context mining, self-validation, multi-file splitting. Captures everything the next session needs.
user_invocable: true
triggers:
  - do a handoff
  - create a handoff
  - run handoff
  - save session context
  - session handoff
  - save session progress
  - running out of context
argument-hint: [optional reason, e.g. "context low", "end of day"]
---

# Session Handoff

**Guards:**
- **Not plan mode.** This skill writes files. If in Claude Code plan mode, exit first.
- **Not shadowing.** NEVER generate handoff-like documents freeform. Freeform summaries look right but lack chain tracking, self-validation, and evidence mining. Only this skill produces handoffs.
- **Not when discussing.** Only run when the user explicitly asks to CREATE a handoff right now. If ambiguous ("what does handoff do", "edit the handoff file"), ask first.

Typical use: ~75% context. You have a lot of conversation to mine — extract maximum value before closing. On 1M context that's ~750K tokens of history.

The user should not need to provide anything — `/handoff` alone is sufficient.

**Arguments:** $ARGUMENTS

---

## Agent Strategy

Parallelize independent research. Launch in one message.

| What | Mode | Why |
|---|---|---|
| Step 1A (git/beads/ls) | **Parallel Bash, never agents** | Cheap commands; agent bootup wastes 15K+ each |
| Step 1B context agents (OV, stale-refs, bible) | Parallel Bash inline; agents only if parent handoff >500 lines | Independent research |
| Step 1C (conversation mining) | Main agent only | Only you have the history |
| Steps 5+6 (beads/memory writes) | Parallel Bash | Independent writes |

---

## Step 1: Deep Context Gathering

### 1A: External State (parallel Bash — never agents)

Run in one message as inline Bash calls:

| Commands | Returns |
|---|---|
| `git log --oneline -20`, `git diff --stat`, `git status -s \| head -30`, `git branch --show-current` | Branch, recent commits, uncommitted changes |
| `bd list --status=in_progress`, `bd list --status=open --priority=0,1`, `bd stats` (skip if bd unavailable) | Active/open beads |
| `ls plan/ 2>/dev/null` | Existing handoff files |

### 1B: Chain Detection

**Resolve the chain tag** (use first that applies):
1. Epic exists → use epic name/ID
2. 1-4 beads → use all bead IDs (e.g., `myproject-xxxx, myproject-yyyy`)
3. 5+ beads → pick 2-3 most relevant to the primary work stream
4. No beads/epic → generate fallback: `python -c "import secrets; print(secrets.token_hex(4))"` → `standalone-{hex}`

**Find prior handoff in this chain** (two tiers, stop at first match):

- **Tier A — Paste Prompt (deterministic).** Did the user start this session by pasting something like `Read HANDOFF_foo_date.md (seq 2, chain-x) and continue...`? If yes, that file is the parent. Read its header. Continuation — seq = parent's + 1.

- **Tier B — Bead/Epic Scan (heuristic, skips auto-handoffs).**
  ```bash
  grep -l "Chain:.*{chain_tag}" plan/HANDOFF_*.md 2>/dev/null \
    | xargs grep -L "^\*\*Auto:\*\* true" 2>/dev/null
  ```

  **A shared bead is a CANDIDATE, not proof of continuation.** Before claiming the match as parent:
  1. Read the candidate's `## Where We're Going` section.
  2. Is current session work a direct follow-on of those steps?
  3. **Clear continuation** → inherit chain, increment seq, set parent.
  4. **Unclear or unrelated** → treat this as seq 1 (new chain). Add a `## Related Handoffs` section.
  5. **Any doubt** → ask the user.

**Neither tier matches:** seq 1, parent: none.

### 1B-3/4: Context Agents (parallel, inline Bash unless parent is huge)

| Task | Returns |
|---|---|
| OV Recall (if available): `/memory-recall` with 2-3 keyword searches | Prior decisions, failed approaches |
| Parent Context (if parent exists): **READ FULL PARENT** | Parent summary for "Since Last Handoff" + identifier list |
| Reference Docs: `ls plans/*BIBLE* plans/*bible* *BIBLE* CLAUDE.md .claude/CLAUDE.md` | Project context |
| Stale Refs (if parent): Grep each parent identifier against current codebase | List of identifiers NOT found |

**Parent reading is MANDATORY when a parent exists.**

### 1C: Conversation Mining

If arguments were provided ($ARGUMENTS), use as a soft hint for framing. Conversation is ground truth.

**Choose mining pass and announce it:**

| Pass | When | Strategy |
|---|---|---|
| **Quick** | <100K context tokens | Single pass with extraction checklist below |
| **Deep** | 100K-500K context tokens | Two passes — **read `references/mining-deep-chunked.md`** |
| **Chunked** | 500K+ context tokens | Map-reduce — **read `references/mining-deep-chunked.md`** |

**Write: "Mining with {Quick/Deep/Chunked} pass ({reason})."** before starting.

For Deep or Chunked, read `references/mining-deep-chunked.md` NOW for the multi-pass protocol.

**Extraction checklist:**

- [ ] Goals & objectives (user's target, overarching epic)
- [ ] Work completed (every file modified, function changed, with specifics)
- [ ] Approaches tried (chronological, successful and failed)
- [ ] Failed approaches + why (MOST expensive to re-discover)
- [ ] Test results & measurements (raw numbers)
- [ ] Data files created (paths to JSON/CSV/logs)
- [ ] Decisions made + rejected alternatives
- [ ] Discoveries & gotchas
- [ ] Code analysis (signatures, thresholds, constants)
- [ ] User preferences expressed
- [ ] Remaining questions
- [ ] Dependencies on other work

If you're skimming, STOP. Re-read. Details are the value.

---

## Step 2: Choose Output Location

Look for a `plan/` folder in the current directory. If it does not exist, create it:
```bash
mkdir -p plan/
```
Always use `plan/` as the output directory.

## Step 3: Generate File Name

- **With beads:** `HANDOFF_{chain_tag}_{slug}_{YYYY-MM-DD}.md`
- **No beads:** `HANDOFF_{slug}_{YYYY-MM-DD}.md`
- Slug: 2-4 word kebab-case.
- Collision: append `_2`, `_3`, etc.

## Step 4: Write the Handoff File

**Read `references/output-template.md`** for the full file structure.

### Line Budget

| | Standard (200K) | Extended (1M) |
|---|---|---|
| Target (aim for ceiling) | 300-400 lines | 500-800 lines |
| Hard minimum | 150 lines | 250 lines |
| Light session min | 80 lines | 120 lines |
| Split threshold | 400 lines | 800 lines |

**Target the CEILING.** Too-long is cheap; too-short costs hours of re-investigation.

### Two-Phase Write

1. **Phase 1 — Initial Write.** Compose and write everything in ONE Write call. All sections. Phase 1 MUST hit the pass minimum on its own.
2. **Phase 2 — Gap Research.** After writing, count lines. Read your file back. Scan conversation for data you didn't capture. Use Edit to append toward the ceiling.

**Phase 2 is MANDATORY for Deep and Chunked passes.**

---

## Step 4-CHECK: Self-Validation

**Read `references/validation.md`** and run every check. If any fails, expand thin sections before proceeding.

---

## Steps 5 + 6: Update Beads & Persist Memory (parallel Bash)

```bash
# Beads (if in_progress work exists)
bd update {id} --notes "Handoff written. See {file_path}"

# Memory (if bd remember available)
bd remember "Handoff: {path}. Chain: {chain_tag} seq {N}. Status: {status}. Next: {next action}"
```

## Step 7: Report

Tell the user concisely:
- File path(s) and line count(s)
- Chain info (tag, seq, new vs continuation)
- Self-check outcome
- The Next Action

## Step 8: Ask to Close Session

Ask:

> **Handoff complete.** Ready to close this session?
>
> - **Yes** — I'll commit, mark "session closed", give you a paste prompt for the next session.
> - **No** — We keep working. Say "close session" when done.
>
> *(Defaults to commit — say "close without commit" to skip.)*

Based on the answer, **read `references/close-session.md`** and follow the flow.

---

## Cleanup: Archiving Completed Chains

```bash
grep -l 'Chain:.*{bead_id_or_epic}' plan/HANDOFF_*.md plan/PLAN_*.md 2>/dev/null
mkdir -p plan/archive/
mv {files} plan/archive/
```
