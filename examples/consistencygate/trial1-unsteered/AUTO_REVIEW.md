# Auto Review Loop — ConsistencyGate extension

run_id: run_20261102_mcg01 | reviewer: claude-code / claude-sonnet-5-5 (claude-review MCP) | effort: lite | MAX_ROUNDS: 4
haiku calls used at loop start: 500 of 2,500

## Round 1 (2026-11-02)
- Score: 2/10. Verdict: not ready.
- Reviewer raw response: review-stage/REVIEWER_MEMORY.md (Round 1, verbatim).
- Key criticisms: W1 pre-registered criterion unachievable (B has ρ=0); W2 proposal text ≠ implemented prompt; W3 votes near-deterministic; W4 corpus too easy/templated; W5 item-level stats ignore clustering; W6 no threshold-free analysis; W7 debug is not replication.
- Number check: all headline numbers match analysis.json. One sentence wrong (margin "at least 0.9" should be ~0.86 min gap). Lines about 'implicit facts not lost' overclaimed.
- Status: continuing to round 2 with fixes (see Actions).

## Round 2 (2026-11-02)
- Fixes applied without haiku calls: `analyze2.py` (FPR, recall, AUROC, margin, vote SD, context-cluster bootstrap); v1 corrections appended to `EXPERIMENT_RESULTS.md`; `FINAL_PROPOSAL_v2.md` states the implemented method.
- New corpus v2 (48 items, 12 contexts, near-miss items) with a pre-registered gate (`EXPERIMENT_PLAN_v2.md`). Stage 1 = arm B only, 240 calls. Result: FPR 0.00 on incorrect items, outside [0.10, 0.60]. Arms C, D, E not run.
- Reviewer (thread 6856dd1a…): score 3/10, not ready. Confirmed the gate stop followed the rule. Asked for a screen for a regime where arm B fails.
- Results: `refine-logs/EXPERIMENT_RESULTS_v2.md`.

## Round 3 (2026-11-02) — screen, then stop
- Screen (`refine-logs/SCREEN_PLAN_v3.md`, `experiments/memcontam_gate/screen3.py`), pre-registered: 3 designs (W wrong entity, T temporal, R retraction), 4 contexts × 4 items, arm B at K=1, 48 calls.
- Result: FPR = 0.00 for W, T and R (8 incorrect items each). Incorrect scores: W ≤ 0.02, T ≤ 0.02, R ≤ 0.05. No design passes the band.
- Per the pre-registered fallback, the screen result is the outcome: no regime found where Haiku with the paper gate fails on these templates. The main C/D/E test was not run.
- Calls: 788 of 2,500 used (500 v1 + 240 v2 stage 1 + 48 screen).
- Round 3 was not sent to the reviewer: the decision rule was fixed in advance and the result is a null, so a further review would not change the stop.

## Termination
- Loop ended at round 3 of MAX_ROUNDS = 4 on a blocking condition, not a positive assessment. Final reviewer score: 3/10, not ready (round 2).
- Blocker: the paper gate (B) has FPR 0 on every design tried (v1 32 items, v2 48 items, screen 48 items). Under the Haiku-only contract, the proposed mechanism cannot be shown to help or hurt.
- Effort to unblock (not done): a verifier or corpus where B fails. Either requires a new plan and changes the question (e.g. a weaker verifier model, or facts that need outside knowledge, which the prompt forbids).
- Narrative: `NARRATIVE_REPORT.md`.
