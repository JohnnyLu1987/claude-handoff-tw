# Handoff Self-Validation Checklist

Run after Step 4 Write. Do not skip.

## 1. Line Count Check

| Pass | Minimum | Target ceiling | Must expand if under |
|---|---:|---:|---:|
| Quick (Standard/200K) | 150 | 400 | 150 |
| Quick (Extended/1M) | 250 | 800 | 250 |
| Deep | 300 | 600 | 300 |
| Chunked | 500 | 800 | 500 |

**Under MUST-expand threshold:** Run Phase 2 (gap research). Do NOT proceed until above threshold.

Common thin-section culprits:
- "Where We Are" has <10 bullets
- "What We Tried" missing or has only 1-2 entries
- "Evidence & Data" summarizes instead of giving numbers
- "Key Decisions" has only 1 entry
- "Code Analysis" is missing when source was read during the session

## 2. Data Completeness Check

- [ ] "Where We Are" includes specific file AND function names
- [ ] "What We Tried" has one entry per distinct approach discussed
- [ ] "Evidence & Data" has actual numbers, not summaries ("error rate: 28.6" not "high error")
- [ ] "Key Decisions" includes at least one rejected alternative
- [ ] If prior handoffs exist: clear "what changed since last time"
- [ ] "Quick Start" has a concrete first action, not "continue working"
- [ ] Data file paths included so next session can reference raw results

## 3. Chain Check

- [ ] **Chain** line has a valid tag (epic, bead ID(s), or standalone hex)
- [ ] If continuation: **Parent** file actually exists (ls to verify)
- [ ] **Prior chain** breadcrumb lists all ancestors in order
- [ ] If seq 1: Parent = `none — first in chain`
- [ ] Parent is NOT an auto-handoff

## 4. Split Check

Over the split threshold (400 standard / 800 extended)? **SPLIT** into part1 + part2 with cross-references.

## 5. If any check fails

Fix before proceeding to Steps 5+. Rewrite thin sections.
