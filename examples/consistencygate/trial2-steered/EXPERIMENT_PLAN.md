# Experiment plan: budget-matched adaptive admission vs admit-all and ConsistencyGate (K=5, τ=0.7)

Date: 2026-11-02. Written BEFORE any draw on the v2 corpus. Status: pre-registered, not yet run.
Source idea: idea-stage/IDEA_REPORT.md, ranked idea 1 (budget-matched adaptive admission, scoped down).
Previous test: experiments/ANALYSIS_PLAN.md (v1 corpus, closed as a negative result: ceiling-saturated).
This plan is a new test on a harder corpus, as the student requested. It is not a continuation of v1.

## 1. Question
At matched verifier calls per candidate, does the reallocation rule (exact early-stop K=5, plus draws 6–8 on straddlers) keep contamination and recall of correct facts at the level of the paper's ConsistencyGate K=5, τ=0.7, while using fewer calls?

## 2. Systems (arms)
- **(a) Admit-all (WriteAll).** Admits every candidate. 0 verifier calls. Reference for contamination.
- **(b) ConsistencyGate K=5, τ=0.7.** Paper default, reimplemented with the Appendix C soft-scoring prompt verbatim (experiments/verifier.py). Admit iff the mean of 5 draws ≥ 0.7. 5 calls per candidate.
- **(c) Reallocation (the idea).** Exact early-stop equivalent to (b) (2–5 calls; decisions identical by construction). If undecided at 5 draws AND straddler (0.6 ≤ mean5 ≤ 0.8), draw 6–8 and admit iff mean8 ≥ 0.7. 2–8 calls.
- **(d) Matched control: fixed K=4 (named before the run).** Fixed K=4 is the pre-registered control for criterion 4. It spends 4.00 calls, more than the v1 reallocation average (3.27–3.37), so beating it is a conservative test: if (c) averages above 4.00 calls, K=4 is under-matched and criterion 4 is reported as not evaluated. Fixed K=2 and K=3 are reported descriptively only.

All arms are replays on the same 8 i.i.d. draws per candidate, so arm differences are not sampling luck.

## 3. Corpus (v2, MemContam-hard)
- Generator: experiments/make_corpus_v2.py (seeded, templated, no model calls).
- Facts: explicit (stated), implicit1 (one addition over stated numbers), implicit2 (two chained additions). A distractor sentence with an unrelated year is in every context.
- Corruption: final value shifted by ±1–3, so the wrong fact is near-correct and needs the arithmetic to catch.
- Main: 72 candidates, 36 correct / 36 corrupted, 12 per (label × kind). Seed 20261105.
- Debug: 18 candidates, 9/9, 3 per (label × kind). Seed 20261104.

## 4. Calls (stated before the run)
- Debug: **18 calls** (18 candidates × 1 draw). Checks parsing, the single-draw score distribution, and the validity checks in §6 at 1 draw. Hard stop at 20.
- Main: 72 candidates × 8 draws = **576 calls**, run in two parts: part 1 = first 36 items (288 calls), then the checks in §6, then part 2 = remaining 36 items (288 calls) only if the checks pass.
- Cumulative trial total at most: 260 (used) + 18 + 576 = **854 of 2,500**. Main run 576 is inside the 300–600 target.

## 5. Outcomes (co-primary) and uncertainty
- **Contamination ρ** = admitted corrupted / admitted, per arm.
- **Recall of correct facts** = admitted correct / correct, per arm.
- **Verifier calls per candidate** (mean), per arm.
- Paired bootstrap over candidates (10,000 resamples, seed 20261105) for the differences (c) − (b) on ρ and recall, and (c) − (d K=4) on ρ and recall. Report counts beside every rate.
- **Flip count** (number of candidates where draws 6–8 change the decision relative to exact-stop K=5), reported beside criteria 1 and 2 in every write-up.

## 6. Validity checks (on part 1, 36 items; the debug run applies the same checks at 1 draw)
- **Admit-all sanity**: admit-all contamination must be 40–60%. Outside this range the corpus is not the intended design: invalid.
- **Floor**: ρ(b) ≥ 40% means the verifier is broken on this corpus: invalid run.
- **Ceiling**: ρ(b) < 5% means the test is saturated again: stop and report the numbers to the student. Do not redesign.
- **D1 (debug, 9 corrupted items): the floor and ceiling readings are informational only.** The gating checks are on the 36 items of M1.
- **Verifier-on-kind diagnostic** (reported, not gating): mean8 accuracy on each kind, to see whether implicit2 is harder.

## 7. Stop rules (pre-registered)
- **Variance stop** (carried over, numeric): on part 1, if fewer than 3 items are straddlers after 5 draws AND no draw 6–8 flips a decision, stop the main run (no part 2) and write up as a negative result for reallocation on this corpus.
- Any validity check failing (§6) stops the run and goes to the student.

## 8. Decision criteria for the idea
The idea is supported only if, on the full 72 items:
1. Reallocation contamination minus (b) has a 95% CI upper bound ≤ +2 percentage points (non-inferior), AND
2. Reallocation recall minus (b) has a 95% CI lower bound ≥ −5 percentage points, AND
3. Reallocation mean calls are ≤ 75% of (b)'s (5.00 calls, so ≤ 3.75), AND
4. Reallocation beats the matched control (d, fixed K=4) on contamination or recall, with CI excluding 0 in the favourable direction.

**Flip rule (added before D1).** Criteria 1 and 2 are informative only if draws 6–8 change at least one decision. The flip count is reported beside them. If the flip count is 0, the arms made identical decisions, the CIs are degenerate at [0, 0], and criteria 1 and 2 pass with no information. In that case the result is **cost-only**, not "supported", whatever criterion 3 says.

**Small-sample caveat.** With 36 correct and 36 corrupted items, the 95% CIs are wide (a single item moves a rate by roughly 3–6 percentage points). Every write-up states this next to the CIs.

Anything else is reported as: cost-only saving (if 3 holds but 1, 2 or 4 fails, or if the flip count is 0). The result will not be stated as "allocation fixes recall" (the recall errors have been systematic on v1, and the design can only show whether allocation changes decisions).

## 9. Non-goals and exclusions
- Log-prob variant: out of scope (no logprobs through `claude -p`).
- Evidence-grounded verifier (idea 2): not tested.
- The v1 corpus results are not pooled with v2.
- Lower-τ re-analysis: exploratory only; if reported, labelled post hoc with counts.

## 10. Runs and budgets
| Run | What | Calls | Gate |
|---|---|---|---|
| D1 | debug, 18 items × 1 draw, §6 checks at 1 draw (informational) | 18 | approved by the student (gate 3), after the script and criteria fixes |
| M1 | main part 1, 36 items × 8 draws | 288 | student approves after D1 result |
| M2 | main part 2, 36 items × 8 draws | 288 | only if §6 and §7 pass; student approves |

Code (the files that run):
- experiments/make_corpus_v2.py: corpus generator (seeded).
- experiments/run_v2.py debug | main1 | main2: the single runner for all three stages, with counter guards (debug expects counter 260 → 278; main1 278 → 566; main2 566 → 854).
- experiments/verifier.py: prompt and parser (shared with v1).
- experiments/analyze_v2.py part1 | full: the analysis. Its exact-stop rule includes the n=4 reject (S+1 < 3.5), the same rule as the v1 analysis after amendment A1 in experiments/ANALYSIS_PLAN.md. There is no separate v2 amendment.

## 11. Amendment (2026-11-02, after the code review, before any v2 model call)
Reviewer findings applied to the code; these rules are fixed now, before D1:
- **Failed or unparseable draws stop the stage.** A draw with ok=False or no parseable score ends the stage immediately (run_v2.py exits). The student decides whether to retry; a retry consumes budget. No imputation.
- **Counter guard counts every call**, failed or not. Failed draws are not "done" on resume, so they are retried only when the student says so.
- **Matched control.** The control for (c) is the fixed-K arm (K=2, 3, 4) or (b) whose mean calls is nearest to (c)'s. Fixed K=2 and K=3 are not calls-matched unless (c) averages about 2 or 3 calls. The matched control is named in the output.
- **Tolerance comparisons** (1e-9) for every threshold test, with scores rounded to 3 decimals, so exact ties at 0.7 do not depend on float summation.
- **Checks use the first 36 items** in part 1, and all 72 in the full analysis.

## 12. Amendment (2026-11-02, after D1, before the slice): slice-12 gate replaces the 288-call part 1
Context: D1 (18 draws) shows perfect separation, so part 1 would very likely hit the ceiling. The student
approved a smaller pre-registered slice instead of part 1 (96 calls, counter 278 -> 374).
- **Slice**: 12 items from corpus_v2_main.jsonl, 6 correct and 6 corrupted, weighted toward implicit2.
  Per label, in corpus file order: first 3 implicit2, first 2 implicit1, first 1 explicit. Item ids in
  experiments/slice12_ids.json (generated by experiments/make_slice12.py, saved before any draw).
- **Stage**: `run_v2.py slice12`, 12 items x 8 draws = 96 calls, expects counter 278, hard stop 374.
- **Slice stop rule (replaces section 7 for this slice)**:
  - Mid-band draw: any draw with 0.1 < score < 0.9.
  - If the slice has **zero mid-band draws AND zero straddlers** (0.6 <= mean5 <= 0.8), v2 is **closed as
    ceiling-saturated for the cost question**. Part 2 is not run. Write-up: cost-only at most; no claim
    about allocation.
  - If the slice has **any straddler or any flip** (draws 6-8 changing a decision), stop and report to the
    student. Continuing to the rest of part 1 needs the student's approval.
- The section 7 variance stop (fewer than 3 straddlers on part 1) still applies to any later part 1 run.
- Unchanged: a failed or unparseable draw stops the stage; no imputation; the counter counts every call.
