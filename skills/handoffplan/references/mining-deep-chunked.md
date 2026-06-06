# Deep and Chunked Mining Protocols

Used for sessions with >100K context tokens. Research shows LLMs have a "lost in the middle" problem — 30%+ accuracy drop for information in the middle of long contexts.

## Deep Pass (100K-500K tokens)

Two passes:

1. **Structured extraction** — Full checklist pass. Force yourself: "Scan the MIDDLE third of the conversation for decisions and measurements I might skip."
2. **Gap-filling sweep** — Review your extraction. Ask: "What from the FIRST HALF is missing? What user feedback from MID-SESSION did I skip?"

## Chunked Pass (500K+ tokens, or 1M context + 50+ tool calls)

Map-reduce — a single pass will miss information at this scale.

1. **Segment** the conversation into 3-4 chronological chunks. Use natural breakpoints.
2. **Per-chunk extraction** — Run the FULL extraction checklist against EACH chunk independently. Tag findings by chunk: `(early/mid/late)`.
3. **Merge + deduplicate** — Later decisions override earlier ones. Build a chronological timeline.
4. **Validation pass** — Ask: "What is missing for a new agent to continue? What comparison tables, cost data, or iteration histories did I skip?"

## Evidence Density Requirements

Chunked pass requires richer Evidence & Data. Heavy sessions produce commit logs, cost tables, approach comparisons, iteration histories, status matrices, and raw data. ALL must be captured.

If your Evidence section has fewer than 3 tables or comparison data sets, you haven't mined deep enough.

## Line Targets

- **Chunked target:** 800 lines (1M context). Minimum: 500.
- **Deep target:** 600 lines. Minimum: 300.

**Phase 1 baseline target (mandatory):**
- Deep: **300-400 lines on first write**
- Chunked: **500-600 lines on first write**

Phase 2 is for additions — tables you skipped, mid-session feedback you missed. Phase 2 is not for filling in sections you left thin.

If Phase 1 lands under its baseline, rewrite Phase 1 first. Don't proceed to Phase 2.

## Anti-Skimming Rule

If you find yourself summarizing instead of extracting, STOP. Re-read the segment. The value lives in specific details — numbers, file paths, function names, exact quotes, error messages with line numbers. Abstract summaries are worth nothing to the next session.
