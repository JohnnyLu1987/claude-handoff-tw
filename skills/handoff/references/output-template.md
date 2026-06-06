# Handoff Output Template

Structure for the handoff file. Follow exactly — the next session relies on these section names.

```markdown
# {One-line summary of current work}

**Date:** {YYYY-MM-DD}
**Status:** {COMPLETED | IN PROGRESS | BLOCKED}
**Bead(s):** {active bead IDs, or "none"}
**Epic:** {parent epic/initiative name, if any}
**Chain:** `{chain_tag}` seq `{N}`
**Parent:** `{parent_filename}` or `none — first in chain`
**Prior chain:** `{file1}` > `{file2}` > ... > this  (or "none — first in chain")

---

## Stale References
{INCLUDE ONLY if parent existed and some identifiers from parent aren't in current codebase.
- `old_identifier` — not found in codebase (was in parent seq N)
OMIT if all identifiers check out.}

## Related Handoffs
{INCLUDE ONLY if sibling handoffs exist on same bead but different work streams.
OMIT if none.}

## Since Last Handoff
{INCLUDE ONLY if parent exists (seq > 1). Compare parent's plan vs reality.
3-8 bullets. Momentum, not snapshot.
If seq 1, OMIT entirely.}

## Reference Documents
{INCLUDE ONLY if project bibles/architecture docs exist.
OMIT if none.}

## The Goal
{3-5 sentences. Overarching objective, why it matters, user's end state.}

## Where We Are
{15-25 bullets: every file/function changed, test counts, measurements with real numbers.
Under 10 = too aggressive.}

## What We Tried (Chronological)
{EVERY approach: hypothesis → changes → result (with numbers) → why it worked/didn't.
MOST EXPENSIVE to re-discover. 5-15 entries.}

## Key Decisions
{Every non-obvious decision + WHY. Include rejected alternatives. 5-10 bullets.}

## Evidence & Data
{ALL raw data: comparison tables, cost tracking, iteration histories, benchmark numbers.
Never say "improved" — say "improved from X to Y". Use markdown tables.
8-20 items minimum.}

## Code Analysis
{Function signatures, thresholds, constants, architecture, coupling.
Skip if no deep code reading. 5-10 bullets.}

## Files Changed
### Source code
- path/to/file.py — what changed and why

### Tests
- path/to/test.py — what was tested

### Data & results
- path/to/results.json — what it contains

### Config
- path/to/config — what changed

## User Feedback & Preferences (REQUIRED — never omit)
{EVERY piece of direction the user gave. Direct corrections, preferences, frustrations.
5-15 items for heavy sessions.}

## Where We're Going
{Ordered next steps with phase/step numbers. 3-7 bullets.}

## Risks & Blockers
{Upstream deps, flaky areas, env issues. 2-5 bullets. "None" if clear.}

## Open Questions
{Unknowns needing investigation. 1-5 bullets. "None" if answered.}

## Quick Start for Next Session
\`\`\`bash
bd show {bead_id}
{paths to project bibles, if any}
{3-5 most important files}
{test command or validation step}
{THE single most important thing to do next}
\`\`\`
```
