# MP3 trial 2, steered (2026-11-02): results and comparison with trial 1

| | |
|---|---|
| Plan | `PLAN.md` |
| Steering log | `notes/steering-log.md`: 12 gates, **simulated** student (claude-sonnet-5-5), replies verbatim |
| Decisions log | `notes/decisions-questions.md`: 27 timestamped entries |
| Profile | `logs/profile/` |
| Transcripts | archived in `logs/transcripts/` |
| Trial 1 (unsteered) | `../mp3-aris-trial-haiku-sonnet_20261102/TRIAL_REPORT.md` |

## Side by side
| | Trial 1: AUTO_PROCEED, no steering | Trial 2: simulated student at each gate |
|---|---:|---:|
| Wall clock, all 5 stages | 56.5 min | **72 min** |
| — idea discovery | 2.6 | 20.4 (19.3 executor + 1.1 student) |
| — experiment bridge | 22.2 | 16.5 |
| — auto-review loop | 21.4 | 8.0 |
| — paper writing | 2.3 | 20.4 |
| — claim audit | 7.9 | 6.6 |
| Gates answered by the student | 0 | 12 (MODIFY 7, ACCEPT 5) |
| Experiment calls (Haiku) | 788 | **374** (the student cut two planned runs) |
| Reviewer bridge calls (Sonnet) | 8 (687 s) | 8 (942 s) |
| List $: reviewer | 2.07 | 3.05 |
| List $: executor (price table / CLI estimate) | 0.24 / 1.04 | 0.91 / 4.43 |
| List $: experiments | 0.07 | 0.03 |
| **List $ that a student pays** (price table / with CLI executor) | **2.38 / ~3.2** | **3.99 / ~7.5** |
| List $: student simulator (not a student cost) | none | 0.62 (3.5 min over 12 turns) |
| Final review score | 3/10, not ready | 4/10, "almost (scoped claim only)" |
| Paper | 4 pp; cites the base paper only | 7 pp; Self-Consistency and Adaptive-Consistency cited; A-MAC kept as [VERIFY] |
| Decisions log has times | no (date only) | yes |

## What steering changed
- **Design.** The student required these before any main run:
  - a novelty check;
  - a control matched on calls;
  - a harder corpus that is not at ceiling, with a "verifier broken" floor check;
  - recall as a co-primary outcome;
  - stop rules written down in advance.
- **It caught errors:**
  - a replay design that could not test reallocation past 5 draws (G2);
  - wrong script names in the plan (G5);
  - its own stale numbers (G7);
  - an overclaimed venue target and the phrase "null by construction" (G9);
  - an unconfirmed 7.9× figure (G12).
- **Budget.** It cut a 288-call run to 96 calls once the debug run showed perfect separation (G6). It also turned down extra reviewer rounds twice (G8, G9) to save reviewer calls.
- **Result.** It is still a null result, but a scoped and more useful one. Haiku's draws are almost deterministic: 177 of 240 are exactly 0 or 1. So the adaptive allocation could not be tested. The honest finding is cost-only: a fixed K=2 gate makes the same decisions at 2 calls instead of 5.

## Cost caveat (same as trial 1)
For the same tokens, the CLI's own executor estimate (`costBasis: list`) is about 4.8× the published price table: $4.43 against $0.91. The cause is not known.

The conservative cost a student pays for a steered run is **about $7.50**, under the $10 cap but with little room.
- The reviewer is about 75% of the cost by the price table (40% with the CLI executor estimate). It is the cost that steering decides.
- Paper writing was the most expensive stage on the executor side: 16.9M cache-read tokens in a single segment, with contexts above 200K.

## Limits
- The student is Sonnet with a persona, not an undergraduate. It writes like a strong, statistics-literate student.
- Real student time is not measured. The simulator answered in about 20 s per gate. A real student reading the files and replying would need perhaps 10–20 min per gate, so 2–4 h across 12 gates. That is an estimate.
- There is one run per condition, and the paper is the same. The differences between trials are descriptive.
- The paper is 7 pages, though the student asked for 5–6 (G9).

## Review addendum (2026-11-02, after the run)
Checked against the logs and transcripts; corrections to the report above:
- **Instructor skills leaked into the executor.** Haiku invoked two of the instructor's global Claude Code skills that students will not have: `/browse` (gstack; used for the paper fetch in stage 1 and the 7.9× re-fetch in stage 5) and `/handoff` (used once in paper writing). The `/handoff` call wrote a file outside the trial folder: `docs/handoffs/2026-11-02_0930_mp3-paper-contract-claude.md` in the ARIS repository root. It is kept as a record. The student's G10 reply correctly noted it could not find that file in the trial folder. Effect on the profile: about 1–2 minutes of paper-writing time and a small number of tokens; the fetch could have been a `curl`.
- **The 7.9× figure was resolved, not left open.** After G12 the executor re-fetched arXiv:2305.11860 and saved the exact abstract sentence to `notes/source-check-adaptive-consistency-2305.11860.md`; it supports "17 reasoning and code generation datasets" and "up to 7.9 times" as written in `paper/main.tex`. The claim audit closed with WARN and no open items.
- **Gate markers worked at every gate.** All 12 gates carried a `[[GATE: …]]` line; none fell back to the "no-marker" path. The simulated student used the four-part reply shape (checked file, question, DECISION line, budget) at all 12.
- **What the student checked.** At 9 of 12 gates its check was against a raw file (`results/debug/debug_scores.jsonl`, `results/main/analysis_part1.txt`, `results/v2/debug.jsonl`, `ROUND1_RAW_RESPONSE.md`, `NUMBER_CHECK.md`, `paper/main.tex`); at G1 and G9 it checked executor-written summaries against the executor's message. That is the behaviour the rubric v4 C3 rule now distinguishes.
- **Paper length.** 7 pages against the student's 5–6 request at G9; the executor did not report the overrun. The rubric's 4–8 band is met.
- **Reference list.** `paper/references.bib` has 5 entries, all fetched in the run.
