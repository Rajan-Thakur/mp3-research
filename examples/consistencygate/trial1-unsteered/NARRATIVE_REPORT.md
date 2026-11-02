# ConsistencyGate extension: narrative report (auto-review loop, 2026-11-02)

Every number below is read from a file in this folder. Files are named in brackets.

## Question
Does a polarity-mixed admission gate (3 support-view + 2 not-supported votes, same 5 calls, τ = 0.7) lower memory contamination compared with the paper's K=5 same-prompt gate, at equal verifier-call cost? The gate is reimplemented; the paper has no public code.

## Outcome: no evidence for or against the proposed mechanism
The test cannot separate the two gates in the regimes tried. The paper gate (B) makes no errors in any of them, so the claim is untestable under the Haiku-only contract.

## What was run
| Stage | Items | Haiku calls | Result | Files |
|---|---|---|---|---|
| v1 main | 32 (8 contexts) | 480 (+20 debug) | B, C, D all FPR 0.00, recall 1.00 | `results/memcontam/analysis.json` |
| v2 stage 1 | 48 (12 contexts, near-miss) | 240 | B FPR 0.00 (0/24), recall 1.00; gate failed, C/D/E not run | `results/memcontam_v2/stage1_items.jsonl`, `results/analysis2_v2.json` |
| v3 screen | 48 (3 designs × 4 contexts × 4 items) | 48 (K=1) | B FPR 0.00 on W, T and R; no design in band | `results/memcontam_v3/screen3_items.jsonl` |

Total haiku calls: 788 of 2,500 (`logs/haiku_calls.count`).

## Verified numbers
- v1 (`analysis.json`): admit-all contamination 0.50 (32 admitted). B, C and D each admitted 16, with contamination 0.00 and recall 1.00. B − admit-all: Δ contamination −0.50, 95% item-bootstrap CI [−0.69, −0.34], exact McNemar p = 3.05e−5. C − B and C − D: zero discordant items.
- v1 vote SD (`analysis2_v1.json`): mean per-item SD 0.0079 (B), 0.0181 (C), 0.0116 (D). The five votes barely vary, so K=5 averaging adds little on these items.
- v1 margin between the lowest correct and highest incorrect mean: 0.888 (B), 0.874 (C), 0.926 (D). AUROC is 1.00 for all three.
- v2 stage 1 (`analysis2_v2.json`): FPR 0.00, recall 1.00, AUROC 1.00, margin 0.88, mean vote SD 0.0062.
- v3 screen (`screen3_items.jsonl`): incorrect-item scores are at most 0.02 for W and T, and at most 0.05 for R.

## Why the test is blocked
1. The paper gate separates correct and incorrect facts with a clear margin on every design: 0.87–0.93 on v1, 0.88 on v2. Its false-admit rate is zero, so a mechanism that lowers contamination has nothing to lower.
2. The votes are almost deterministic (SD ≈ 0.01), so the premise that averaging K samples reduces noise is barely exercised here.
3. Three further designs (wrong-entity, temporal, retraction) were screened at one vote per item. None made the baseline fail.

## Limits
- Ground truth is set by construction, so it is only as good as the templates. The templates are simple and the same author wrote them.
- The verifier is Haiku only. Results with the paper's own verifier (Qwen2.5-32B), and with a weaker verifier, are not tested here.
- The screen used K = 1. The v1 and v2 runs used K = 5, which did not change the zero-error result.
- The reviewer reviewed the plan, code and results (round 1, round 2). Round 3 (the screen) was not reviewed, because its pass rule was fixed beforehand and it returned a null.

## Corrections to earlier writing
- The v1 sentence "margin of at least 0.9" is corrected in `EXPERIMENT_RESULTS.md` (appended section): the measured margins are 0.87–0.93.
- The v1 "implicit facts were not lost" claim is overclaimed. The v1 implicit item is a trivial inference. v2 uses multi-hop items, and the baseline still admits all of them (scores 0.90–0.99).
- The v1 B−A result is a comparison against a no-op baseline. It confirms the paper's basic finding and does not test the proposal.
- The stage-1 refutation wording ("contradiction only") was superseded before the v1 run. Results describe the implemented "not supported" prompt. See `FINAL_PROPOSAL_v2.md`.

## Reviewer assessment
- Round 1: 2/10, not ready (pre-registered criterion unachievable; proposal and implementation disagreed; corpus too easy).
- Round 2: 3/10, not ready. The stop at the gate followed the pre-registered rule. The reviewer asked for a screen for a regime where the baseline fails. Verdict unchanged.
- Thread: `6856dd1a-530a-4485-9bae-7697b4bd4604`. Raw responses are in `review-stage/REVIEWER_MEMORY.md`.

## What would settle it (not done)
A regime where the paper gate makes errors on a verifier the contract allows, followed by the arms C, D, E at 5 calls per item. A weaker model would be a different experiment, and so would facts needing outside knowledge, which the prompt forbids. Either needs its own plan, and both change the question.

## Files
- Plans: `refine-logs/EXPERIMENT_PLAN.md`, `refine-logs/EXPERIMENT_PLAN_v2.md`, `refine-logs/SCREEN_PLAN_v3.md`
- Results: `refine-logs/EXPERIMENT_RESULTS.md` (with appended corrections), `refine-logs/EXPERIMENT_RESULTS_v2.md`
- Proposal: `refine-logs/FINAL_PROPOSAL.md` (original), `refine-logs/FINAL_PROPOSAL_v2.md` (as implemented)
- Review: `review-stage/AUTO_REVIEW.md`, `review-stage/REVIEWER_MEMORY.md`, `review-stage/REVIEW_STATE.json`
