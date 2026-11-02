# Steering log (trial 2): SIMULATED student

Every student reply below was written by claude-sonnet-5-5 playing an IPHS 400 student (tools/student_prompt.md). It is not an undergraduate. Each entry: gate, the student's reply verbatim.


## G1 2026-11-02 08:37 | 10_idea-discovery | gate: ref paper summary

Simulated student (claude-sonnet-5-5) reply, verbatim:

Your summary matches what I understand, and I checked it against `REF_PAPER_SUMMARY.md`. The 0.58 recall, the 34.1% contamination and the Table 3 [VERIFY] flag all match your message. I like that you left the two Table 3 rows unquoted.

I have two concerns about budget allocation.

1. **Is the gap real?** Your own line 29 calls early stopping "an exact way to save calls, not a new mechanism." Adaptive or sequential self-consistency may already exist. Before I commit, I want a novelty check with arXiv IDs you actually fetched.
2. **Would it fix the weak regime?** The 0.58 recall on LoCoMo-Contam comes from evidence spread across many turns. That looks like a systematic verifier miss, not sampling noise. If the 5 samples all agree on a wrong low score, spending more samples on "ambiguous" cases won't help. Do you have evidence that those failures are high-variance? Without it, the claim that this targets the weakest regime is unsupported.

**Decision:** MODIFY. Pursue budget allocation, but frame it as a cost-versus-accuracy question, not a recall fix.
- The controls are the paper's fixed K=5 gate and a plain fixed K=2 (or K=3) gate at matched average calls. Without that matched control, a saving doesn't show the allocation rule matters.
- The harder corpus is required, not optional. At the current 1.2% contamination the test is at ceiling and can't fail. I want a check, before the main run, that K=5 on the harder corpus is clearly below ceiling.
- Report paired uncertainty, such as a paired bootstrap over items.
- State that the log-prob variant is out of scope.

**Budget:** the literature and novelty check uses 0 experiment calls. The next step may use at most 20 calls, for a debug run that measures how many candidates are ambiguous.

DECISION: MODIFY — Keep budget allocation, but add a novelty check, a matched fixed-K control and a harder non-ceiling corpus, and don't claim it fixes recall.

## G2 2026-11-02 08:39 | 10_idea-discovery | gate: novelty result and debug-run approval

Simulated student (claude-sonnet-5-5) reply, verbatim:

I opened `IDEA_REPORT.md`. The novelty table and the rule on lines 32–33 match your message. The narrowed framing is honest, and I accept it.

**A flaw in the design.** Lines 48–49 draw 5 samples per candidate and replay every policy on prefixes of those draws. Lines 32–33 say the exact early-stop rule matches K=5 decisions, so it only saves calls. The part that could be new is spending the saved calls on straddling candidates, which means going beyond 5 samples. A replay of 5 draws cannot show what a 6th or 7th sample would do. So the contamination-at-matched-cost test can't be run as written. Which is it: do you draw extra samples for straddlers (say, up to 8 per candidate), or do you drop the reallocation claim and report only the cost saving? I'd draw a capped set of extra samples for candidates that straddle τ after 5. Keep this inside the 2,500 cap, and state the total before the main run.

**Question 2, the stop rule.** The 5% threshold at 30 items is noisy, because it rests on about 15 corrupted items. Keep the rule, but also report the contaminant count and make the corpus 50/50. Add a floor check: if K=5 contamination is near 50%, the verifier is broken or the prompt is wrong, and the run is also invalid.

**Question 1, the debug run.** The 20-call debug run is approved. Use 10 clean and 10 corrupted candidates, mixing explicit and implicit facts, and keep the generator and seed saved first.

**Budget:**
- Debug: 20 calls, hard stop.
- The main run waits for my confirmation of the debug result.
- Fetch Appendix C before the main run, so the baseline prompt isn't a deviation.

DECISION: MODIFY — Approve the 20-call stratified debug and the narrowed framing, but fix the replay design so reallocation past 5 samples can be tested, and add a floor check to the stop rule.

## G3 2026-11-02 08:42 | 10_idea-discovery | gate: debug result and main-run approval

Simulated student (claude-sonnet-5-5) reply, verbatim:

I checked `results/debug/debug_scores.jsonl`. The counts match: 10 scores at 0.0, 5 at 1.0, 3 at 0.5, and one each at 0.9 and 0.3. The one wrong rejection is `debug-1-imp-03`, scored 0.0 with label 1.

**What the file shows**
- All 10 corrupted items scored 0.3 or below. At a single sample, no contaminant would be admitted.
- All 3 mid-band scores are correct implicit facts at 0.5. Both of the other two correct implicit facts, 0.9 and 0.0, are also at the extremes.
- So contamination looks near the floor. The ceiling check will probably fire. The weak point is recall on correct implicit facts, which is also where the paper reports its weakness. A test that only measures contamination may not be able to fail.

**Question 1: main run.** The 576-call run is approved in stages, with a modification:
- Run the first 30 items (240 calls) in shuffled order, so they stay about 50/50.
- Make the primary outcome contamination and recall of correct facts together, with a paired bootstrap on each. If contamination is at the floor, recall is where arms can differ.
- Report counts next to every rate.
- If the ceiling rule fires, stop and bring me the numbers. Do not redesign the corpus on your own.

**Question 2: the extra stop rule.** "About zero" is vague, so define it before you look. Stop if, among the first 30 items, fewer than 3 are straddlers after 5 draws and no draw 6–8 would flip a decision. That outcome is itself a negative result: draws are near-deterministic, so allocation has nothing to allocate. Write it up as such, with the draws in hand.

**Generator fix:** disclosure accepted. The corrected log entry is the right call.

**Budget:** 240 calls now, 596 cumulative at most. Remaining items need my go after the first 30.

DECISION: MODIFY — Approve the staged main run (first 30 items, 240 calls), add recall as a co-primary outcome since contamination is likely at the floor, and give the variance stop rule a numeric definition.

## G4 2026-11-02 08:52 | 10_idea-discovery | gate: close as negative result or redesign

Simulated student (claude-sonnet-5-5) reply, verbatim:

I checked `results/main/analysis_part1.txt` against your table. The numbers match: 0/12 corrupted admitted in every arm, recall 70.6%, mean calls 3.27 and 3.37, one straddler and zero flips. The five K=8 errors are all correct implicit facts. Three of them have draws stuck at exactly 0.5 (main-1-imp-09, imp-13 and imp-16 at SD 0.033).

**Question 1: close as a negative result?** Yes. The test could have failed. A straddler with a draw 6–8 flip would have separated the arms, and there were none. The cause is that the errors are systematic, so redesigning the corpus would test a different question. I reject Q2 as a redesign of the allocation test. I won't spend calls to hunt for variance that isn't there.

**Things to put in the write-up**
- The one positive result is a cost result. Exact-stop gives the same decisions as K=5 at 3.27 mean calls against 5.00. That is a saving of about 35%, with no change in decisions. It is a sequential-bound argument, not a new mechanism. It was measured on 30 items, one verifier and one corpus, and the draws were bimodal, so I would not generalize it.
- The CIs of exactly 0 are not evidence of equivalence. Say plainly that the arms never disagreed.
- The 17/13 imbalance is fine, but state it.
- Skipping items 31–72 is a pre-registered stop. Say so.

**An exploratory idea, 0 calls:** the 0.5 draws look like a stable "can't tell" mode. On the draws already saved, re-score recall and contamination offline at a lower τ (for example 0.5). Label it post hoc and exploratory, not a new method, and report it with counts. This costs no calls.

**Budget:** 0 further experiment calls. Fetch A-MAC and Self-Consistency to close the citations. Use one reviewer call on the finished report, and give it the raw files.

DECISION: ACCEPT — Close the allocation test as a negative result and report the 35% cost saving as cost-only. Run a free offline τ re-analysis labelled exploratory, then one reviewer pass.

## G5 2026-11-02 09:00 | 20_experiment-bridge | gate: approve D1 debug run and decision criteria

Simulated student (claude-sonnet-5-5) reply, verbatim:

I read `refine-logs/EXPERIMENT_PLAN.md`. Most of it is good. The four arms, the matched control, the "no redesign if ceiling" rule and the no-imputation rule are all sound. I found three problems.

1. **The plan names the wrong scripts.** §10 says `debug_v2.py` and `main_draws_v2.py` are "to be written", and mentions an amendment A1 (n=4 reject). Your message says `run_v2.py` and `analyze_v2.py` exist, and the plan has no A1 (only §11). Fix the file names so I know which code ran.
2. **Criteria 1 and 2 can pass for nothing.** Arm (c) is identical to (b) by construction except where draws 6–8 flip a decision. If there are no flips, the paired CI is exactly [0, 0], and non-inferiority "passes" with no information, as it did on v1. Add to §8 that the flip count is reported beside criteria 1 and 2. If there are zero flips, the result is "cost-only", not "supported".
3. **Criterion 4 can't be met cheaply.** Fixed K=2 and K=3 aren't calls-matched to (c). That is addressed in your amendment, but name the matched control before the run, not after seeing results.

**Decision criteria.** I confirm 1–3 with those caveats. The +2 pp and −5 pp bounds are reasonable for 36 correct and 36 corrupted items, but note that a 36-item arm gives wide CIs and say so in the write-up.

**D1.** Approved at 18 calls (hard stop at 20). Treat its ceiling and floor readings as informational only. With 9 corrupted items, the gating checks are on the 36 items of M1.

**Budget:** D1 is 18 calls. M1 needs my approval after I see the D1 numbers.

DECISION: MODIFY — Run the 18-call D1 as planned, and before it runs, fix the script names, name the matched control, and make criteria 1–2 report flip counts so a no-flip result counts as cost-only.

## G6 2026-11-02 09:03 | 20_experiment-bridge | gate: approve M1 part 1

Simulated student (claude-sonnet-5-5) reply, verbatim:

I checked `results/v2/debug.jsonl`, and your description is right. All 18 draws are 0.0 or 1.0. The 9 corrupted items all scored 0.0, and the 9 correct items all scored 1.0. That includes the implicit2 items, so the harder corpus did not make the verifier unsure.

Spending 288 calls to confirm this looks like a waste. The plan's ceiling rule is evaluated on 36 items, but D1 already shows perfect separation, so the full part 1 would almost surely hit the ceiling and the variance stop. The calls aren't scarce (278 of 2,500), but the same test isn't informative. I'm also not going to ask for a third corpus redesign, because that would be fishing for variance.

**Decision:** MODIFY, with a smaller, pre-registered M1.
- Run a slice of **12 items × 8 draws = 96 calls** (278 → 374). Stratify it to 6 correct and 6 corrupted, weighted toward implicit2.
- Before running, amend §7 of the plan. If that slice has no draw between 0.1 and 0.9 and no straddlers, v2 is closed as ceiling-saturated for the cost question, and part 2 is not run. If any straddler or flip appears, continue to the rest of part 1, with my approval.
- Keep the plan's rule that a failed draw stops the stage.

**What the write-up should say:** Haiku saturates on near-correct numeric corruptions in templated text. The calls saved by early-stopping are real, because bimodal draws stop at 2–3. But a test where the arms can't differ doesn't support the claim that allocation matters. Also state that the 18-draw D1 is not enough to make that claim, which is why we run the 12-item slice.

**Limitation to record:** an easy verifier on a synthetic corpus says nothing about messy real memories, which is where the paper's recall problem sits.

DECISION: MODIFY — Run a 96-call, 12-item slice with a pre-registered ceiling stop instead of the 288-call part 1, because D1 shows perfect separation.

## G7 2026-11-02 09:16 | 30_auto-review-loop | gate: round 1 review, choose fixes

Simulated student (claude-sonnet-5-5) reply, verbatim:

I read `ROUND1_RAW_RESPONSE.md` and checked the cost numbers against `results/main/analysis_part1.txt`. The current file gives exact-stop 3.17 and reallocation 3.27. The old `analysis_part1_before_n4_fix.txt` gave 3.27 and 3.37. I had quoted the old numbers earlier, so the corrected file is the one to use. Say in the write-up that the earlier figures predate the n=4 bound fix.

**Review points I agree with**
- The reviewer's strongest point is the cost baseline. Fixed K=2 matches exact-stop's recall and contamination at 2.00 calls against 3.17, so "K=5 is wasteful on a near-deterministic verifier" is the honest finding, not "early-stop saves 37%".
- The 36.6% figure belongs to exact-stop only, and the 34.6% to reallocation. These are arithmetic facts, not findings.
- Also true: K=1 admitted imp-00 on one 0.7 draw, so "arms never disagreed" must go.

**Question 1: which fixes?** I accept A, C and D. I accept B with a limit: the v1 miss breakdown covers only 5 items, so report counts and the arithmetic direction (backward versus forward) descriptively and make no causal claim. It costs 0 calls and directly addresses the reviewer's point that v2 removed the failure mode.

**Additions to the write-up**
- The saving formula and its dependence on corruption prevalence.
- 0 errors in 15 corrupted items has an upper bound of about 20%, not "perfect separation".
- The slice was a data-informed deviation, written down before the run.

**Question 2: round 2?** Yes, but only a re-check of the revised write-up. It is one reviewer call, with no new corpus, no new experiments and no extra calls, so the counter stays at 374. If round 2 still disagrees with a number, fix the number and don't argue with the reviewer.

DECISION: ACCEPT — Apply A–D (B descriptive only) at zero calls, put the K=1 to 5 ladder at the center of the write-up, then one reviewer re-check.

## G8 2026-11-02 09:20 | 30_auto-review-loop | gate: round 3 re-check or close the loop

Simulated student (claude-sonnet-5-5) reply, verbatim:

I checked the text myself in `idea-stage/IDEA_REPORT.md`. Line 130 now says fixed K=2 gives the same decisions as exact-stop (2.00 against 3.17 calls), and that K=1 disagrees with K=5 on `main-1-imp-00`. It also says all five v1 misses are backward subtractions and that this is descriptive only. Line 125 labels the τ=0.5 result post hoc. The framing is honest.

**Decision: close now (option 2).** A round 3 would spend one scarce reviewer call to check five wording fixes that I can see in the file. Round 2 already verified the numbers against the raw draws, and the score is capped by the data, not the prose. Record the loop as stopped at round 2, 4/10, with the same-family/provisional label.

**`NARRATIVE_REPORT.md` must carry**
- **Claim:** allocation couldn't be tested because the verifier is near-deterministic. K=5 is wasteful here. A fixed K=2 or K=1 matches the decisions at 2 and 1 calls, and the 34.6% and 36.6% savings are arithmetic facts on a 50/50 mix.
- **Not claimed:** any improvement in contamination or recall.
- **Limitations:** one verifier (Haiku), templated synthetic text, 30 + 18 + 12 items, v2 removed the v1 failure mode, 0/15 gives a ~20% upper bound, and the slice stop rule was under-powered.
- **Alternatives and future work:** the τ=0.5 re-analysis on the saved draws, the backward-subtraction family tested with a fresh pre-registration, and an evidence-grounded verifier. All three are unrun.
- **Disclose:** the earlier 3.27 and 3.37 figures were corrected to 3.17 and 3.27, and the slice was a data-informed deviation.

**Budget:** no further Haiku calls and no more reviewer calls. The counter ends at 374 of 2,500.

DECISION: ACCEPT — Close the loop at round 2 (4/10, provisional) with no more calls, and write `NARRATIVE_REPORT.md` as a scoped negative result plus a cost-only note.

## G9 2026-11-02 09:25 | 40_paper-writing | gate: paper plan framing and Phase 1.5 approval

Simulated student (claude-sonnet-5-5) reply, verbatim:

I checked `PAPER_PLAN.md` against the saved results. Several numbers recompute from the raw data. C6 matches the score distribution in `analysis_part1.txt`: 99 + 78 = 177 of 240 draws at 0 or 1 (74%), and 39 at 0.5 (16%). The C9 bounds are right: 0 of 15 gives about 18%, 0 of 9 about 28% and 0 of 28 about 10%.

**Question 1: framing.** I choose Option A. The evidence is a null test plus a cost note. A methods paper would need a corpus or verifier where draws 6–8 change decisions, and we never found one. The title and abstract already name the scope and the non-claims, which is the right balance. I'm modifying it in two ways:
- Treat this as a short empirical note. The plan budgets 8 pages, but 42 items and one verifier don't support that. Aim for 5–6 pages: drop the optional Fig 2, move Table 2 to the appendix, and keep Fig 1, Table 1 and Fig 3. Also drop "ICLR (target)", because a venue claim over-states what this is. Say workshop or tech-note scope.
- Cut "null by construction" in the C5 wording. The test could have shown a difference if draws had varied, and the stop rules were pre-registered. "Null on these draws" is accurate.

**Question 2: Phase 1.5.** I approve it, but cap it at **one reviewer round**, not three. The plan is already 6/10 with no blocking error, and the remaining risk is wording. If the one round finds a number problem, fix it and don't negotiate further.

Keep [AMAC] marked [VERIFY], and keep the "cite only fetched sources" rule.

**Budget:** 0 Haiku calls, and 1 reviewer call at most.

DECISION: MODIFY — Choose Option A (null test plus cost note) and approve Phase 1.5 with one reviewer round. Shrink it to a 5–6 page note, drop the ICLR claim and the optional Fig 2, and replace "null by construction".

## G10 2026-11-02 09:32 | 40_paper-writing | gate: contract re-check and approval to write the figures and note

Simulated student (claude-sonnet-5-5) reply, verbatim:

Most of what you report checks out in the files. `PAPER_ACCEPTANCE_CONTRACT.md` A6 and A9 and `PAPER_PLAN.md` C3 and C4 agree on 95/150 (36.7%), 98/150 (34.7%) and f = 12/30. The formula (1 + 2f)/5 also gives 36.0% for f = 0.4 and 40% for f = 0.5. Your correction of the 37.3% and 36.6% figures is right.

Two things I can't confirm:
- **The handoff.** A Glob for `docs/handoffs/*` in this folder found nothing. Confirm the full path of `2026-11-02_0930_mp3-paper-contract-claude.md` and that it exists before `/clear`.
- **The file that changed on disk.** I made no edit to the contract, so I have no edit for you to reconcile. But a file that changed unexpectedly before you overwrote it should be written in `notes/decisions-questions.md`, with what you saw.

**Question 1: reviewer re-check of the contract?** No, I'm skipping it. The reviewer's demands were all applied, and I checked the cost numbers against the raw draws. Instead, spend one reviewer call at the end, on the compiled PDF against the contract. That tests the actual deliverable.

**Question 2: figures and LaTeX?** Yes, with these rules:
- Build only from the saved draws, with no model calls.
- Fig 3 must plot v1 as exact-stop (36.7%), labelled as a formula, with the text saying "consistent with", not "validated".
- Table 2 goes to the appendix.
- [AMAC] stays marked [VERIFY], and the bibliography comes only from the five fetched sources.
- The note is 5–6 pages, and the abstract states the scope (one verifier, 42 templated items) and the non-claims.
- Run a final check of every number in the PDF against the raw files.

**Budget:** 0 Haiku calls and 1 reviewer call at the end.

DECISION: ACCEPT — Skip the contract re-check, build the figures and the 5–6 page note from saved draws, then use one reviewer call on the compiled PDF.

## G11 2026-11-02 09:40 | 40_paper-writing | gate: approve the compiled note or request changes

Simulated student (claude-sonnet-5-5) reply, verbatim:

I'm checking the compiled note's source and the number check before I decide. I've read `NUMBER_CHECK.md` and lines 44 and 104 of `paper/main.tex`.

**What checks out**
- The `NUMBER_CHECK.md` entries agree with what I verified earlier: 95/150 and 98/150, 177 and 39 of 240 draws, K=1 at 13/17 against K=5 at 12/17, and the bounds of 18.1%, 28.3% and 10.1%. The 34.6% and 36.6% lines are gone from the derived file.
- The Limitations paragraph says what I want it to say. It covers one verifier, the same-family review, reimplementation, small samples, the bound caveat, untested independence, the under-powered slice and the missing log-prob variant.
- Line 44 states that items 31–72 were not run, and that the slice was approved before it ran.

**Two small things**
1. Line 44 says the debug set "showed perfect separation". That is literally 18 of 18 at one draw each, so it's defensible, but I'd write "all 18 single draws separated the labels" to match how we handled that wording earlier.
2. `NUMBER_CHECK.md` says the family breakdown counts and the admit-all baselines (13/30, 6/12) were checked by hand only. Put them through the script too, or say in one line that they stay unscripted.

**Decision:** approve the note with those two small edits. The overfull line is cosmetic and can stay. I accept your rejection of the two reviewer claims about the 28% bound and the 273/336 pool, since both are in the derived file.

**What I'd keep as the lasting message:** this is a scoped null test plus a cost note, one verifier and 42 items, with no claim about contamination or recall. The v2 corpus removed the failure mode it was built to test, and the τ=0.5 result is post hoc. No more reviewer calls and no Phase 6 audit, because the assurance level is `draft`. The counter stays at 374.

DECISION: ACCEPT — Approve the compiled note, with the "perfect separation" wording softened and the unscripted counts either scripted or flagged. No further calls.

## G12 2026-11-02 09:46 | 50_paper-claim-audit | gate: paper claim audit — verdict WARN, 4 fixes applied, 7.9x open

Simulated student (claude-sonnet-5-5) reply, verbatim:

Before I approve, I checked the evidence trail, and it is inconsistent. `idea-stage/IDEA_REPORT.md` line 24 says the 7.9x figure came from the Adaptive-Consistency abstract page I fetched earlier, with "17 datasets". You now say the only source is a search snippet. `paper/main.txt` line 45 adds "on reasoning and code", which neither version supports. Which one is true, and where is the fetched text saved?

I don't want to cite a number whose source I can't point to.

**Decision on the 7.9x claim:** re-fetch, don't just mark it. This costs no experiment calls.
- Fetch the arXiv abstract page for 2305.11860 once, respecting the 3-second rule, and quote the exact sentence into `notes/`.
- If the page confirms "up to 7.9x" and the 17 datasets, keep the number with only the wording the page supports. Drop "reasoning and code" unless the page says it.
- If the fetch fails or the figure doesn't appear, delete the number and keep the qualitative statement that it stops sampling when the answer votes agree. Don't leave an unverified figure in the text, even with a `[VERIFY]`, since the point of the sentence is only related work.
- Rebuild the PDF and recheck that no other number has a source gap.

**The four fixes:** I accept all four. Items 2 and 3 matter most, since they disclose that the slice rule and the K=3 control were fixed after earlier data. That is what keeps the pre-registration claim honest.

**Audit:** WARN is the right verdict. Approve it after the 7.9x fix. No reviewer calls, no Haiku calls, and the counter stays at 374.

DECISION: MODIFY — Re-fetch the abstract page to confirm 7.9x with its exact wording, delete the figure if it can't be confirmed, then approve the WARN audit.
