## Week 10 — Iteration & reflection

### Reviewer feedback

**Feedback received:** [ ] Yes  [X] No — still awaiting review

**Summary of feedback:**
No reviewer or maintainer feedback came in during the contribution cycle.
Per the Su26 course note, reviewer feedback is not a feature this term.

**How you responded:**
N/A — no feedback to respond to. If the PR gets reviewed later, I'll
address comments then.

---

### Reflection

**What was harder than you expected?**
The writing wasn't the hard part — making the doc actually match the code
was. The thing that took longest was the score normalization. The vector
side already comes out between 0 and 1 (the vector store converts distance
to a similarity with 1/(1+distance)), but the BM25 keyword scores are
unbounded, so the two aren't on the same scale at all. Before they get
weighted, each set gets divided by the top score in its own result set to
squash it into 0–1, and only then does it blend them 0.7 vector / 0.3
keyword. I assumed at first there was a fixed scale somewhere, and it took
re-reading retrieve() a few times to realize the normalization is relative
to whatever the max score happens to be that query.

**What did you learn about working in a large codebase?**
On my own projects I already know where everything is, so I never have to
read code this closely. Documenting someone else's code is a higher bar —
you have to understand it well enough to explain it to the next person, not
just enough to run it. Reading that carefully surfaced stuff I'd have
skipped over otherwise: the vector store creates its collection in cosine
space but the query code comments that ChromaDB distances are euclidean and
converts them with 1/(1+distance), and retrieve() fetches all_chunks but
then never actually uses it. I also confirmed the real weights (0.7/0.3)
straight from the constructor defaults instead of guessing, which was
exactly the thing I flagged as an open question back in Week 7.

**How did AI tools help — and where did they fall short?**
AI was solid for the general concepts — explaining how BM25 and vector
similarity get combined in hybrid retrieval, and cleaning up how I wrote
the formula. Where it fell short was anything specific to this repo. It
tends to assume a "standard" hybrid setup, like the vector store handing
back a clean cosine similarity, when this code actually does a euclidean-
style 1/(1+distance) conversion and then max-normalizes each result set
against its own top score. If I'd trusted the generic explanation the doc
would've described a formula that doesn't match what the code does. I had
to read the source to get the real version.

**What would you do differently if you started over?**
I'd confirm the exact weights and the normalization scheme in the code
*before* drafting the explanation, instead of drafting first and going back
to verify. In Week 7 I even wrote down "need to confirm the exact weighting"
as an open question, then kind of wrote around it for a while. Pinning the
implementation details down first would've saved me a couple of rewrites.

**What are you most proud of from this module?**
That the blending math is actually written down now. Issue #37 existed
because the scoring formula was undocumented, and the non-obvious part —
that scores get normalized against each result set's max before the 0.7/0.3
weighting — wasn't written anywhere. Turning that into something a future
contributor can just read, instead of having to trace hybrid.py themselves
like I did, feels like a real (if small) improvement to the project.
