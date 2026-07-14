# Journal

## Week 7

**Issue:** #37 — Architecture doc doesn't explain the hybrid retrieval scoring formula
**Branch:** docs/37-hybrid-retrieval-scoring-doc
**Repo:** https://github.com/soccerthomas/pathreview

### Goal
Document the hybrid retrieval scoring formula in the architecture docs so contributors
understand how keyword and vector similarity scores are combined and weighted.

### Plan
- Read the retriever module to find where hybrid scoring is implemented
- Trace how keyword and vector similarity scores are normalized and combined
- Write a clear explanation with the formula and an example into the architecture doc

### Progress this week
- Forked and cloned the repo, set up the local environment (.venv)
- Created working branch docs/37-hybrid-retrieval-scoring-doc off main
- Added this JOURNAL.md as the initial setup commit

### Blockers / Questions
- Left a comment on the issue asking if it's still open (previous claim looked stale)
- Need to confirm the exact weighting used in the hybrid scoring code

### Next steps
- Locate and trace the scoring formula in the retriever
- Draft the architecture doc section