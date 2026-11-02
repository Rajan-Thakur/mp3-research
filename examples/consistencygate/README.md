# MP3 worked example: ConsistencyGate, the base paper and two pipeline outputs

This folder lets you put the original paper next to what the MP3 pipeline produced from it, twice, on November 2, 2026. ConsistencyGate is the instructor's example and **may not be your base paper** (manual §2.1).

| Folder | What it is |
|---|---|
| `base-paper/` | The original: Zhang et al., *ConsistencyGate: Preventing Memory Contamination in LLM Agents via Self-Consistency Admission Control*, arXiv:2607.22962 v1 (July 25, 2026), 24 pages. `2607.22962v1.pdf`, the arXiv HTML rendering `2607.22962v1.html`, and the abstract page `abs_2607.22962.html` with the submission history. Licence on arXiv: non-exclusive distribution 1.0, which permits arXiv to distribute it; the copies here are for this course's private site only. Cite and link the arXiv page, not this folder. |
| `trial1-unsteered/` | Pipeline run with no human at the gates (`AUTO_PROCEED: true`): `paper.pdf` and `paper.tex` (4 pages, one reference), `NARRATIVE_REPORT.md`, and the reviewer's rounds in `AUTO_REVIEW.md` (final 3/10, "not ready"). Executor Haiku 5.5, reviewer Sonnet 5.5, 56 minutes, 788 experiment calls. |
| `trial2-steered/` | The same pipeline with a student answering at 12 gates: `paper.pdf`, `paper.tex`, `references.bib` (7 pages, five fetched references), `NARRATIVE_REPORT.md`, `AUTO_REVIEW.md` (final 4/10, "almost, scoped claim only"), the claim audit `PAPER_CLAIM_AUDIT.md`, and `steering-log_SIMULATED.md`. Same models, 72 minutes, 374 experiment calls. |

**Read the steering log for its shape, not its words.** The "student" in trial 2 was Claude Sonnet playing a student. The rubric gives such text no C3 credit, and the grader reads your raw transcript. What to copy is the four-part reply: check one number in a raw file, ask the question that could make the test fail, decide with a reason that adds something, set the budget for the next step (manual §5.3).

**What to look for when you compare the three papers**
- The original reports contamination dropping from 50% to about 1.2% on a synthetic corpus and gains on two conversation corpora, with K=5 verifier calls per fact.
- Both trial papers reimplemented the gate (no public code) and found Haiku's verifier almost deterministic on templated facts, so the proposed changes could not beat the baseline: null results at the ceiling. Trial 2's paper says why, reports counts beside every rate, and lands on a cost-only finding (a fixed 2-call gate makes the same decisions as the 5-call gate). Trial 1's paper reports the null with less scoping and one reference.
- Neither trial did the human audit of Parts 7–8 or wrote a REPORT.md, and neither used `paper/paper.md`; a student submission has those. The graded scores of both trials against the rubric are in `../../mp3-what-to-expect.md`.
