# Paper claim audit report

**Paper**: A null test of adaptive admission for ConsistencyGate on a near-deterministic verifier (paper/main.tex)
**Auditor**: claude-sonnet-5-5 via the claude-code route. Fresh thread deefd96c-4ef0-4ec9-8e5e-dc17840421b7, read-only (Read, Grep, Glob). Same family as the executor (Haiku). Label: same-family / provisional.
**Skill note**: the skill's default reviewer is a Codex route. This audit used the claude-code route, as the student's invocation requested.
**Scope deviation**: the student wrote "refine-logs/". Only `refine-logs/EXPERIMENT_PLAN.md` (the pre-registration) was passed. `refine-logs/EXPERIMENT_RESULTS.md` is an executor summary and is excluded by the skill.

## Overall verdict: WARN (reason_code `scope_wording`)

- No number mismatch was found. Every count, mean, call total, bound and percentage matched the raw draws or the derived file.
- Five wording issues were found. All five were fixed in `main.tex` after verification (see below).
- The one external number (Adaptive-Consistency "7.9x") was re-fetched and reworded to match the abstract (see Open items, now closed).

## Verification performed after the reviewer reply

The reviewer's claims were checked against the raw files before any edit:

- **Stop counts (cost paragraph).** Recomputed from `results/main/draws_part1.jsonl` with the exact-stop rule. Call distribution: 2 calls ×12, 3 calls ×2, 4 calls ×15, 5 calls ×1, total 95. All 12 two-call items are corrupted. The 3-call items are `main-0-imp-04` (corrupted) and `main-1-imp-06` (correct). The 5-call item is `main-1-imp-00` (correct). The paper's text ("two items at three calls, one corrupted and one correct; one correct item took five") is correct. The reviewer's labels were also correct.
- **Admit-all baseline.** `results/derived_numbers.txt` lines 15–16: 13/30 corrupted (v1), 6/12 corrupted (slice).
- **Matched control.** `refine-logs/EXPERIMENT_PLAN.md` §2d names fixed K=4 as the pre-registered control. §11 (amendment, before any v2 model call) replaces it with the nearest-call fixed-K arm, which is K=3 here.
- **Stop-rule timing.** `refine-logs/EXPERIMENT_PLAN.md` §12 (amendment, after D1, before the slice) fixes the slice stop rule after the 18-draw debug set.
- **Admit-all in arms.** The paper's Setup lists admit-all as arm (a). Admit-all admits corrupted items, so the Results sentence "every arm admitted no corrupted item" was false as written.

## Issues found and fixed

| # | Location | Paper said | Evidence | Status | Fix applied |
|---|---|---|---|---|---|
| 1 | Results, Decisions | "Every arm admitted no corrupted item on the 42 evaluated items" | admit-all admits 13/30 and 6/12 corrupted (`derived_numbers.txt`) | scope_overclaim → fixed | "Every gated arm admitted no corrupted item …; admit-all admits 13 of 30 (v1) and 6 of 12 (slice) corrupted items." |
| 2 | Intro, Scope | "The stop rules were fixed before any v2 draw." | plan §12: slice stop rule fixed after D1, before the slice | scope_overclaim → fixed | "The v2 plan was fixed before any v2 draw; the slice stop rule was fixed after the 18-draw debug set and before the slice draws." |
| 3 | Setup, Arms | fixed K=3 is the nearest comparison; no mention of the earlier control | plan §2d (K=4) replaced by §11 (K=3) | disclosure gap → fixed | Added: the pre-registered control was fixed K=4; the §11 amendment replaced it with the nearest-call arm, K=3. |
| 4 | Setup, Corpora | "the pre-registered ceiling rule … and variance stop rule both fired" | rules are in `ANALYSIS_PLAN.md`, written before analysis, not before collection | wording → fixed | Now reads "the ceiling rule … and the variance stop rule, both from the v1 analysis plan (written before analysis, not before collection)". |

## Not changed (checked, acceptable)

- **Table 1 cost cells** "3.17 (95/150)" and "3.27 (98/150)". Reviewer: ambiguous mapping. The numbers are right: 95/30 = 3.17, 98/30 = 3.27. The 150 is the K=5 budget. Left as is.
- **All other claims** (C1–C35, C37–C38; reviewer's table): exact_match or rounding_ok. These include the 177/240 and 39/240 draw counts, 273/336 pooled, 96/96 slice draws in {0,1}, the bounds 18.1%, 28.3% and 10.1%, the tau=0.5 exploratory numbers, the call total 374 = 20+240+18+96, and the before/after costs.

## Open items (closed)

1. **Adaptive-Consistency 7.9x: closed.** Fetched https://arxiv.org/abs/2305.11860 via /browse on 2026-11-02. The abstract reads: "Our experiments over 17 reasoning and code generation datasets and three LLMs demonstrate that Adaptive-Consistency reduces sample budget by up to 7.9 times with an average accuracy drop of less than 0.1%." The sentence in `main.tex` now says "reduces the sample budget by up to 7.9 times across 17 reasoning and code generation datasets". Quote saved at `notes/source-check-adaptive-consistency-2305.11860.md`. Correction to the earlier version of this report: the 7.9x figure was not sourced only from a search snippet. The same abstract was also in an earlier fetch logged at `logs/stages/10_idea-discovery_s1.jsonl`.
2. **Other external numbers: none.** A grep of `main.tex` found no other external citation figure (the 0.58 LoCoMo figure is not in the paper).
3. **Reviewer notes** (kept for the record, no effect on results).

Superseded text from the first version follows, for the record only:

(superseded) 1. **Adaptive-Consistency "7.9x" (Related work).** The paper says "reports sample-budget reductions of up to 7.9x on reasoning and code benchmarks". The only fetched evidence in this folder is a search snippet (`logs/stages/10_idea-discovery_s1.jsonl`) reading "7.9 times ... on 17 datasets". The snippet does not confirm the "reasoning and code" scope, and the full paper was not re-fetched in this stage. Per the citation rule, this should carry `[VERIFY]` until checked. Not changed here; it needs a student decision (mark `[VERIFY]`, or fetch and confirm).
2. **Reviewer notes.** The reviewer reported that plan mode blocked file edits. It made no edits and its findings were checked independently, so this has no effect on the results.

## Edits and checks

- `paper/main.tex` edited in four places (issues 1–4). Rebuilt with `latexmk -pdf -interaction=nonstopmode main.tex`: exit 0, 7 pages, zero undefined references (`main.log`).
- Logged in `notes/decisions-questions.md`.
- Hashes of the audited inputs are in `PAPER_CLAIM_AUDIT.json` (`audited_input_hashes`), taken after the edits.
