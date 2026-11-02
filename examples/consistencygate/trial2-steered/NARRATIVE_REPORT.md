# Narrative report: budget-matched adaptive admission for ConsistencyGate

Status: scoped negative result, plus a cost-only note. Closed 2026-11-02 at review round 2 (4/10, "almost", same-family / provisional). Every number below comes from a file in this folder; see the sources in brackets.

## Claim
A reallocation rule that spends extra verifier draws on straddling candidates could not be tested for its effect on contamination or recall, because the verifier is near-deterministic on these corpora. The test had no decisions to change. K=5 is wasteful here: a fixed K=2 gives the same decisions at 2 calls, and K=1 matches the decisions at 1 call except for one item. The reported savings are arithmetic facts about the calls, not evidence that the method helps. [`idea-stage/IDEA_REPORT.md`, `refine-logs/EXPERIMENT_RESULTS.md`, `results/ladder_and_breakdown.txt`]

## What was run
- **Method under test:** exact early-stop K=5 (same decisions as the paper's ConsistencyGate, K=5, τ=0.7), plus draws 6–8 for straddlers (mean of draws 1–5 in [0.6, 0.8]). The verifier prompt is the paper's Appendix C soft-scoring prompt, verbatim. The reimplementation is ours. The arXiv HTML page has no code link; the abstract says the gate is released, which I did not verify [VERIFY].
- **Arms:** admit-all; ConsistencyGate K=5; the reallocation rule; fixed K=1 to 5 (the ladder).
- **Data:** v1 corpus, first 30 items × 8 draws (17 correct, 13 corrupted); v2 corpus, 12-item slice × 8 draws (6 correct, 6 corrupted) and D1, 18 items × 1 draw (9 correct, 9 corrupted). Corpora are seeded, templated and labelled by construction; no model call touched a label. [`experiments/make_corpus.py`, `experiments/make_corpus_v2.py`]
- **Calls:** 374 of 2,500 Haiku calls in total (20 v1 debug + 240 v1 main = 260; 18 for D1; 96 for the slice). [`haiku` counter; `logs/haiku_calls.jsonl`]

## Results
Contamination is admitted corrupted items divided by admitted items. Recall is admitted correct items divided by correct items.

| Setting | Arm | Contaminated / admitted | Recall | Mean calls | Disagree with K=5 |
|---|---|---|---|---|---|
| v1, 30 items | fixed K=1 | 0 / 13 | 13 / 17 | 1.00 | 1 (`main-1-imp-00`) |
| v1, 30 items | fixed K=2 | 0 / 12 | 12 / 17 | 2.00 | 0 |
| v1, 30 items | fixed K=3 | 0 / 12 | 12 / 17 | 3.00 | 0 |
| v1, 30 items | fixed K=4 | 0 / 12 | 12 / 17 | 4.00 | 0 |
| v1, 30 items | fixed K=5 | 0 / 12 | 12 / 17 | 5.00 | — |
| v1, 30 items | exact-stop K=5 | 0 / 12 | 12 / 17 | 3.17 (95/150) | 0 |
| v1, 30 items | reallocation | 0 / 12 | 12 / 17 | 3.27 | 0 |
| v2 slice, 12 items | fixed K=1 to 5; exact-stop; reallocation | 0 / 6 | 6 / 6 | 1.00 to 5.00; 3.00; 3.00 | 0 |
| v2 D1, 18 items | fixed K=1 (one draw) | 0 / 9 | 9 / 9 | 1.00 | no K=5 reference |

Other observations, all from the saved draws:
- All 96 slice draws were 0.0 or 1.0. There were no mid-band draws (0.1 < score < 0.9) and no straddlers, so the reallocation rule never spent a draw beyond 5 on the slice. [`results/v2/analysis_slice12.txt`]
- The one v1 straddler, `main-1-imp-00`, had a 5-draw mean of 0.62 and an 8-draw mean of 0.575. It was rejected under both rules. [`results/main/analysis_part1.txt`]
- The earlier figures 3.27 and 3.37 for exact-stop and reallocation predate the bound fix at n=4. The current file gives 3.17 and 3.27. [`results/main/analysis_part1_before_n4_fix.txt`]

## Savings (arithmetic, not findings)
- Exact-stop K=5 saves 36.7% of the calls against fixed K=5 on the v1 slice (95 of 150 calls). The reallocation rule saves 34.7% (98 of 150 calls). Both figures come from the v1 slice, which is about 43% corrupted (13 of 30), not from a 50/50 mix.
- With bimodal draws, a reject stops at 2 calls and an admit at 4. The saving is therefore about 20% + 40% × (corrupted fraction). That is 40% at 50/50, and about 24% at 10% corruption. This prevalence dependence is untested at realistic rates.
- Fixed K=2 gives the same decisions as exact-stop at 2.00 calls on this data, so early-stop is not the cheapest option here. The sequential bounds that justify early-stop are a known argument (Adaptive-Consistency, arXiv:2305.11860).

## Not claimed
- No improvement in contamination or recall for the reallocation rule. Its decisions matched K=5 on every item.
- No conclusion that allocation matters. The arms could not differ on these draws.
- No claim that K=5 is better or worse than K=1 or K=2. K=1 has one extra admission (`main-1-imp-00`, one lucky first draw of 0.7 on an item whose 8-draw mean is 0.575), and its recall is slightly higher than K=5's at equal contamination.
- No claim about the paper's recall regime. The paper's LoCoMo-Contam recall drop (0.58 in its Table 1) is multi-turn and was not reproduced.

## Limitations
- **One verifier:** Haiku (claude-haiku-5-5), with the Sonnet reviewer from the same family. Any review positive is same-family / provisional.
- **Synthetic, templated text:** three families, one or two arithmetic steps, and a distractor sentence. Corruptions shift a single number.
- **Small samples:** 30 v1 items (17/13), 18 D1 items (9/9), 12 slice items (6/6). The 0-error counts give one-sided 95% upper bounds on the error rate of about 18% for 15 corrupted items and about 28% for 9.
- **Sampling assumption:** the draws were treated as independent in the plan. Independence was not tested. Within-item draws are near-deterministic (for example, every draw of `main-1-imp-09` was 0.5).
- **Fresh corpus, no fresh signal:** v2 removed the failure mode seen in v1. Its correct implicit items all scored 1.0 on every draw, against 3 of 8 correct implicit items admitted on v1. v2 therefore did not reproduce the v1 recall failure.
- **Slice stop rule under-powered:** the 12-item slice closes the test only for those 12 items. With a 10% straddler rate, a 12-item slice would show none about 28% of the time.
- **Descriptive family breakdown only:** all five v1 rejections of correct implicit facts are backward subtractions (bio and project), and the three forward items were admitted. Five misses on eight items supports no causal claim. Family and direction are confounded in v1.
- **Deviation disclosed:** the 12-item slice replaced the planned 288-call part 1 after the 18-call D1 showed perfect separation. The slice was written down and approved before it ran.

## Alternatives and future work (none run)
1. **τ re-analysis on the saved draws:** the τ=0.5 result (16/17 recall, 0/16 contaminated at K=5, but 1/17 contaminated at K=1) is post hoc and exploratory. It needs a fresh pre-registration before any claim. [`results/main/tau_reanalysis_exploratory.txt`]
2. **Backward-subtraction family:** a fresh pre-registration on a corpus where the arithmetic direction is varied and the family is held fixed, to test the descriptive pattern.
3. **Evidence-grounded verifier:** retrieve the sentence that mentions the key entity before scoring. This changes the verifier, so it is not a same-cost comparison.
4. **Real conversational memory:** the paper's regime is multi-turn dialogue, where the recall problem sits. None of this work tests that regime.

## Review record
- Round 1: 3/10, not ready. Fixes A–D (fixed-K ladder; descriptive breakdown; wording; additions) were applied at zero calls. [`review-stage/ROUND1_RAW_RESPONSE.md`]
- Round 2: 4/10, almost. The reviewer recomputed the ladder, breakdown, savings and bounds against the raw draws and found them correct. Five wording fixes were applied at zero calls. [`review-stage/ROUND2_RAW_RESPONSE.md`]
- Loop stopped at round 2 by the student's decision. The score is below the threshold of 6. Reviewer: claude-sonnet-5-5 via the claude-code route, same family as the executor. Label: same-family / provisional. No round 3.

## Disclosures
- Earlier exact-stop and reallocation costs (3.27 and 3.37) predate the n=4 bound fix and are superseded by 3.17 and 3.27.
- The 36.7% and 34.7% savings (exact integer calls, 95/150 and 98/150) are arithmetic on the v1 slice, not findings. Earlier rounded values (36.6%, 34.6%) came from rounded means.
- The slice was a data-informed deviation from the pre-registered part 1, written down before the run.
- The "i.i.d." wording in the plan was an assumption, not a tested property.
