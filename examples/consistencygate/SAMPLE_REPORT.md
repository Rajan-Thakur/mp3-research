# MP3 Final Report (SAMPLE): ConsistencyGate, written by the instructor from the two trial runs

> **Read this before copying anything.** This sample fills the report template from the instructor's two pipeline trials on November 2, 2026, with trial 1 as run A and trial 2 as run B; sections 5b and 5c were added when the written report replaced the presentation. The "student" in run B was a model (Claude Sonnet) playing a student, so section 3 shows the *shape* of a steering account and would earn 0 authorship points if a student submitted it. Every pointer below is into `examples/consistencygate/` or the trial folders it was copied from. Your report must point into your own repository.

## 1. The paper and why I chose it (C1, eligibility)
- Base paper: *ConsistencyGate: Preventing Memory Contamination in LLM Agents via Self-Consistency Admission Control*, Zhang et al., arXiv:2607.22962, v1 2026-07-25, 24 pages, code: none (gate reimplemented).
- Class topic: externalizing memory to files / verification (a write-time admission gate for an agent's memory store).
- Mechanism in one sentence: before a candidate fact is written to memory, ask the verifier K=5 times for a 0–1 support score and admit the fact only if the mean is ≥ τ=0.7.
- Paper test: passes (a) date and (b) one mechanism; **fails the 6–9 page guideline** (24 pages) and has no public code, which is why it is now excluded for students. It was chosen before the page rule existed. The second-check critique would have said: USE WITH THIS SCOPE CUT, reimplement only Algorithm 1 on a synthetic corpus.
- Gap chosen (run B): the paper spends five identical verifier calls per fact; can the saved calls be reallocated to facts whose score straddles τ? Evidence: `trial2-steered/NARRATIVE_REPORT.md` §Question; the paper's §4.2 single-pass variant shows the authors also cared about call cost.

## 2. The two runs (T1, T4)
| | Run A: unguided | Run B: guided |
|---|---|---|
| Command and session ID | `/idea-discovery … AUTO_PROCEED: true`; session `757ee34c…` (`trial1-unsteered/TRIAL_REPORT.md`) | `/idea-discovery … AUTO_PROCEED: false, human checkpoint: true`; session `669f020e…` (`analysis_trial1-vs-trial2.md`) |
| Agent wall time | 56.5 min | 72 min (executor) + 3.5 min of student replies |
| Experiment calls / reviewer calls | 788 / 8 | 374 / 8 |
| API spend (list-price equivalent) | $2.40–3.20 | $4.00–7.50 |
| Gates answered | 0 | 12 |
| Outputs | `trial1-unsteered/` | `trial2-steered/` |

## 3. How I steered run B (C3)
The three decisions a student would list, taken from `trial2-steered/steering-log_SIMULATED.md` (G2, G6, G7). Shown in the required block shape; the pointers go to the simulated log, so this section is **not** a model of authentic authorship.

### Decision 1 (idea stage): the replay design could not test the idea
- Raw pointer: `steering-log_SIMULATED.md:27-44` (G2)
- Agent proposal (quoted): "draw 5 samples per candidate and replay every policy on prefixes of those draws"
- My question: a replay of 5 draws cannot show what a 6th or 7th sample would do; which is it, extra draws for straddlers or drop the reallocation claim?
- DECISION: MODIFY — draw a capped set of extra samples (up to 8) for straddling candidates; add a "verifier broken" floor check to the stop rule.
- Budget allowed: 20 calls for the debug run; main run waits for my confirmation.

### Decision 2 (experiment stage): cut the main run after the debug run showed perfect separation
- Raw pointer: `steering-log_SIMULATED.md:111-130` (G6); checked `results/v2/debug.jsonl` (all 18 draws exactly 0.0 or 1.0)
- Agent proposal: run part 1 of M1, 288 calls
- DECISION: MODIFY — run a pre-registered 12-item × 8-draw slice (96 calls) instead; if no draw lands between 0.1 and 0.9, close v2 as ceiling-saturated and do not run part 2.
- Budget allowed: 96 calls (278 → 374 of 2,500).

### Decision 3 (review stage): accept the reviewer's cost-baseline point and my own stale numbers
- Raw pointer: `steering-log_SIMULATED.md:134-154` (G7); checked `results/main/analysis_part1.txt` (3.17 / 3.27 calls, not 3.27 / 3.37)
- Agent proposal: four fixes A–D from reviewer round 1
- DECISION: ACCEPT — apply A–D at zero calls, make B descriptive only, put the K=1…5 ladder at the centre of the write-up, then one reviewer re-check.
- Budget allowed: 0 experiment calls, 1 reviewer call.

What run B changed relative to run A: a novelty check, a call-matched control (fixed K=2, K=3), recall as a co-primary outcome, written stop rules, and half the experiment calls. Cost: about 15 minutes more agent time and roughly $1–4 more, almost all reviewer tokens.

## 4. Judge scores for run A and run B, and why they differ
Scores below are from the instructor's GPT-6.1 grading of both trials (`trial1-unsteered/GRADE_rubric-v4.md`, `trial2-steered/GRADE_rubric-v4.md`), the items a student self-judges.

| Item | Run A | Run B | Where the difference comes from |
|---|---:|---:|---|
| F1 Baseline | 5 | 5 | both reimplemented the K=5 gate correctly (`paper.tex`, `experiments/`) |
| F3 Controlled comparison | 3 | 5 | B added the call-matched fixed-K control and reported counts beside rates (`trial2-steered/EXPERIMENT_PLAN.md:12-15`) |
| T3 Stage handoffs | 4 | 4 | both produced usable idea report, plan, review, narrative |
| T5 Numerical provenance | 4 | 4 | both traced; B added `NUMBER_CHECK.md` |
| D1 Reference identity | 4 | 6 | A's single bibitem omitted the first author; B's five entries all resolved (`trial2-steered/references.bib`) |
| D2 Citation support | 3 | 3 | both supported the base-paper claims they made |
| C1 Gap and novelty | 4 | 6 | B's novelty check found Adaptive-Consistency and bounded the claim (`trial2-steered/NARRATIVE_REPORT.md`) |
| C2 Method contribution | 6 | 7 | B's control distinguished the mechanism from "any extra draws" |
| C4 Critical appraisal | 5 | 5 | both listed alternatives and threats |
| Sum (of 48) | 38 | 45 | |

Why they differ: every B gain traces to a gate decision. The F3 and C2 points came from Decision 1 (extra draws, matched control); the C1 points from the novelty check demanded at G1; the D1 points from the "cite only fetched sources" rule at G4 and the 7.9× re-fetch at G12. Where B is not better: the result itself. Both runs are null at the ceiling, because Haiku's verifier gives almost only 0 or 1 scores on templated facts (177 of 240 draws). The ceiling check that would have caught this before the main run is now a required step (manual §6.2).

## 5. What my human review found in the AI-written draft (D3, X1, X2)
From `trial2-steered/PAPER_CLAIM_AUDIT.md` and the paper-writing audit (`analysis_trial1-vs-trial2.md`):
- Numbers that did not trace: the abstract said 80 items; the raw files hold 128 (32 + 48 + 48). Fixed. "At most 0.01" was really 0.012; changed to "at most 0.02".
- References: the 7.9× figure for Adaptive-Consistency was cited from a search snippet; re-fetched the abstract page, which does say "up to 7.9 times" across "17 reasoning and code generation datasets"; kept with the page's wording (`trial2-steered/decisions-questions.md`). A-MAC and MemGuard appeared only in snippets and were not cited.
- Claims wider than the evidence: "reproduced", "harder designs", "capable verifier" and "perfect separation" were softened to what 18 single draws and 42 items support; "null by construction" became "null on these draws".
- AI-slop passages: a reference to a non-existent "gate E"; a venue claim ("ICLR") that overstated what a 7-page note is. Removed.
- Checks that found nothing: K=5 and τ=0.7 match the paper's §4.1; the paired bootstrap recomputes from `results/main/analysis_part1.txt`.

## 5b. Design explanations, written (R2)
- **Why this method change:** the paper spends five identical verifier calls on every fact, including facts whose first draws are already 0.0 or 1.0; reallocating draws to straddlers should save calls where they are wasted and spend them where a decision can flip (`trial2-steered/NARRATIVE_REPORT.md` §Question). Trade-off: it only helps if draws vary; on a deterministic verifier it changes nothing, which is what happened.
- **Why these controls and this budget:** admit-all gives the floor, the paper's K=5 gate is the baseline, and fixed K=2/K=3 are call-matched controls so a saving can be attributed to the rule rather than to fewer calls (`trial2-steered/EXPERIMENT_PLAN.md:12-15`). Budget: 30 items × 8 draws, then a pre-registered 96-call slice instead of 288 calls once the debug run showed perfect separation (G6). Trade-off: a 12-item slice is under-powered, stated as a limitation.
- **What ARIS and the reviewer did, and why I kept or changed it:** the reviewer's round-1 point that fixed K=2 matches exact-stop's decisions at 2.00 calls (`trial2-steered/AUTO_REVIEW.md` round 1) was accepted and became the paper's central table; its request for a third round was declined because the fixes were wording I could verify in the file (G8). Kept: the same-family "provisional" label.

## 5c. Evidence questions, answered in writing (R3)
- **Q1.** Decision 2 above: the agent proposed the 288-call part 1; I checked `results/v2/debug.jsonl` (18 of 18 draws exactly 0.0 or 1.0) and cut it to a 96-call pre-registered slice. Bearing: it saved 192 calls and turned a likely ceiling repeat into a stated stop rule.
- **Q2.** Weakest point: every corpus was templated synthetic text on which Haiku is almost deterministic, so the allocation idea was never actually tested. The test that would settle it: the same four arms on a conversation corpus like the paper's LoCoMo-Contam (recall 0.58 in the paper); five or more discordant items would make the question answerable.
- **Q3.** Abstract: "fixed K=2 matches K=5 at 2.00 calls against 3.17". Source: `results/main/analysis_part1.txt` (exact-stop mean calls 3.17 = 95/150 draws over 30 items; K=2 = 2.00 by construction); recall 12/17 for both (`NUMBER_CHECK.md`).

## 6. Result and limitations (C2, C4)
- Result: the reimplemented gate removed all 16 incorrect facts at full recall against admit-all (Δ contamination −0.50, 95% CI [−0.69, −0.34], 0 vs 16 discordant). The proposed reallocation changed 0 decisions in 42 items (paired CI [0, 0], 0 discordant), so it could not be evaluated. Cost-only finding: fixed K=2 matches K=5 decisions at 2.00 calls per fact instead of 5.00.
- What the test could not show: anything about facts the verifier is unsure of, because none occurred.
- Alternatives not taken: an evidence-grounded verifier (changes the mechanism, not the budget); a harder near-miss corpus (tried once in B's review loop, still saturated). Threats: templated text inflates verifier certainty (direction: makes every gate look perfect); one verifier model (direction: unknown). Next experiment: the same arms on a conversation corpus like the paper's LoCoMo-Contam, where the paper reports recall 0.58; an outcome with ≥5 discordant items would make the allocation question testable.

## 7. Budget and integrity
- Spend (list-price equivalent, Max plan in the trials): run A $2.40–3.20, run B $4.00–7.50; experiment calls 788 + 374 of a 2,500 cap; Pro limit stops: none.
- AI use: Haiku 5.5 executor and experiment model, Sonnet 5.5 reviewer, Sonnet 5.5 as the simulated student (run B), GPT-6.1 Sol as the grader of both runs; the instructor's assistant (Claude) wrote this sample from the trial files.
- What went wrong: one automatic context compaction in run B's last stage (no loss); the executor used two instructor-only skills that a student will not have (`analysis_trial1-vs-trial2.md`, review addendum).
