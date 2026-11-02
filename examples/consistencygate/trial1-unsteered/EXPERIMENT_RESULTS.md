# Experiment Results: polarity-mixed ConsistencyGate, synthetic MemContam-style corpus

**Plan**: `refine-logs/EXPERIMENT_PLAN.md` (pre-registered success criterion; written before any run)
**Raw data**: `results/memcontam/main_items.jsonl`, `main_calls.jsonl`, `analysis.json`; debug run: `results/memcontam/debug_*.jsonl`
**Code**: `experiments/memcontam_gate/{corpus,gate,analyze}.py`
**Calls**: debug 20 + main 480 = 500 of the 2,500-call trial cap. 0 parse failures, 0 timeouts, 0 missing votes.

## Result: inconclusive. The test sits at the ceiling, so the proposal cannot be separated from the baseline.

| Arm | Admitted (of 32) | Contamination ρ | Recall | Verifier calls / fact |
|---|---|---|---|---|
| A. admit-all | 32 | 0.50 | 1.00 | 0 |
| B. paper gate, K=5 τ=0.7 (reimplemented) | 16 | 0.00 | 1.00 | 5 |
| C. polarity-mixed (proposed) | 16 | 0.00 | 1.00 | 5 |
| D. paraphrase control | 16 | 0.00 | 1.00 | 5 |

Admit rate by item class (16 correct, 16 incorrect):

| Arm | correct explicit (8) | correct implicit (8) | contradicted (8) | unsupported (8) |
|---|---|---|---|---|
| A | 1.00 | 1.00 | 1.00 | 1.00 |
| B | 1.00 | 1.00 | 0.00 | 0.00 |
| C | 1.00 | 1.00 | 0.00 | 0.00 |
| D | 1.00 | 1.00 | 0.00 | 0.00 |

Paired contrasts (bootstrap over items, 10,000 resamples, seed 20261102; exact McNemar on admission discordance):

| Contrast | Δ contamination [95% CI] | Δ recall [95% CI] | discordant items | McNemar p |
|---|---|---|---|---|
| C − B | 0.00 [0.00, 0.00] | 0.00 [0.00, 0.00] | 0 vs 0 | 1.0 |
| C − D | 0.00 [0.00, 0.00] | 0.00 [0.00, 0.00] | 0 vs 0 | 1.0 |
| B − A (paper gate vs admit-all) | −0.50 [−0.69, −0.34] | 0.00 [0.00, 0.00] | 0 vs 16 | 3e−5 |

Score ranges per class (mean of the arm's votes; threshold 0.7):

| Arm | correct explicit | correct implicit | contradicted | unsupported |
|---|---|---|---|---|
| B | 0.98–1.00 | 0.91–0.97 | 0.00–0.02 | 0.00–0.02 |
| C | 0.98–1.00 | 0.92–0.97 | 0.00–0.03 | 0.00–0.05 |
| D | 0.99–1.00 | 0.93–0.97 | 0.00–0.01 | 0.00 |

## Reading
- **Pre-registered criterion not met.** The proposal needed ρ(C) < ρ(B) with a CI excluding 0. The observed Δ is exactly 0, so the idea shows no improvement here. This is a null result for the proposal on this corpus, not evidence against it.
- **Ceiling.** Every class separates by a margin of at least 0.9 on the 0.7 threshold, and the paper's baseline already gets ρ = 0 at 100% recall. A test that cannot show a difference in either direction says nothing about polarity.
- **Admit-all vs paper gate** is the one real effect here: the gate removes all 16 incorrect facts, and the Δ is significant (p = 3e−5). This only confirms the paper's MemContam finding, with Haiku as verifier.
- **Implicit facts were not lost.** The recall concern from stage 1 (refutation views rejecting implicit facts) did not show up: C admitted all 8 correct-implicit items, with the minimum score 0.92.
- The debug run (20 calls, arm C, context c0) agrees with the main run.

## Limits (state them with any number above)
- 32 items from 8 contexts, built by the same author who wrote the proposal. Ground truth is set by construction, so it is only as good as the templates.
- All distractors are easy: an unsupported fact is an obviously unrelated claim. Near-miss facts are missing, which is the case the idea was meant to help with.
- One sample per vote, one model (Haiku). Sampling noise is not quantified beyond the 5 votes.
- The reviewer reviewed the code before the run (no CRITICAL findings; fixes applied). The results were not reviewed by the reviewer (lite effort).
- The stage-1 refutation-prompt wording was changed before the run (see `notes/decisions-questions.md`).

## Next step (not run)
A harder corpus, with near-miss unsupported facts (plausible paraphrases of source facts, with the wrong relation), and paraphrase-correct facts to test recall. Then re-run arms B, C, D on the new items. That needs a new plan and budget entry; it would use roughly 500 more calls (about 1,000 of 2,500 total).

## Corrections after loop round 1 (appended; the text above is unchanged)
- The margin sentence is wrong. The minimum gap between the lowest correct mean and the highest incorrect mean is 0.87 (C) to 0.93 (D), not "at least 0.9 on the threshold". Measured from `main_items.jsonl`: B 0.888, C 0.874, D 0.926.
- "Implicit facts were not lost" is overclaimed. The v1 "implicit" item is a trivial two-city inference. See `refine-logs/EXPERIMENT_RESULTS_v2.md`.
- The debug run is a partial repeat on the same items (c0-0 to c0-3), not an independent replication.
- Vote SD is tiny: mean per-item SD is 0.008 (B), 0.018 (C), 0.012 (D). The five votes barely vary, so K=5 averaging adds little here (`analysis2_v1.json`).
- The "contradiction-only" wording in the proposal does not match the implemented prompt. Results describe the implemented version (NS prompt). See `refine-logs/FINAL_PROPOSAL_v2.md`.
