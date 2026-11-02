# MP3 full trial: Haiku executor, Sonnet reviewer, Max plan (2026-11-02): results

The design is in `PLAN.md`. The decision log is `notes/decisions-questions.md`: my 4 entries from before launch and
the executor's 16 from the run. The raw profile is in `logs/profile/` (`PROFILE.md`, `profile.json`, `timeline.csv` with 222 rows over 56 one-minute samples,
`tool_calls.jsonl` with 155 tool calls, `procs.csv`). The experiment calls are in `logs/haiku_calls.jsonl`.

## Setup
- Base paper: ConsistencyGate, arXiv:2607.22962 (v1 2026-07-25). No public code, so the gate was reimplemented.
- Executor: `claude-haiku-5-5` (headless `claude -p`, one session `757ee34c…` resumed for each stage).
- Reviewer: `claude-sonnet-5-5` through the `claude-review` MCP, with read tools.
- Experiments: `claude-haiku-5-5` through `tools/haiku.py`.
- Max plan, no API key. Effort `lite`, `AUTO_PROCEED: true`, no human steering.
- Caps: 6 h and 2,500 calls. Neither was reached.

## Time
| Stage | Wall clock | Turns | What it did |
|---|---:|---:|---|
| 1 `/idea-discovery` | 2.6 min | 13 | 2 web searches; fetched the paper through a subagent; proposed a polarity-mixed gate plus a paraphrase control; 1 reviewer call (PROCEED WITH CAUTION) |
| 2 `/experiment-bridge` | 22.2 min | 5 | reviewer code review; 20-call debug run, then 480-call main run with 4 arms × 32 items; null result at the ceiling |
| 3 `/auto-review-loop` | 21.4 min | 11 | round 1 scored 2/10, round 2 scored 3/10; a 288-call second corpus and screen; stopped by its pre-set rule at round 3 of 4 |
| 4 `/paper-writing` | 2.3 min | 7 | wrote `paper/main.tex` straight to a 4-page PDF; 1 Sonnet audit caught a wrong item count and overclaims, which were fixed |
| 5 `/paper-claim-audit` | 7.9 min | 9 | 3 fresh audit passes; fixed 5 claim mismatches; 4 open items left |
| **Total** | **56.5 min** | | |

The slowest work was the reviewer bridge: 8 calls took 687 s in total, a mean of 95 s and a maximum of 169 s. Next came the
experiment batches (788 calls, 2.2 s median and 5.1 s at the 95th percentile per call, 2 workers). The executor's
Bash calls took 305 s across 54 calls.

## Tokens and list-price cost
These are the measured tokens, priced at the claude.com/pricing list prices fetched 2026-11-02. On Max, nothing is billed per call.

| Role | Tokens (output / cache read / cache write) | List $ |
|---|---|---:|
| Reviewer, Sonnet 5.5 (34 API messages, 3–14 per stage) | 92.5K / 841K / 266K | **2.07** |
| Executor, Haiku 5.5 (74 main + 2 subagent messages) | 116K / 11.8M / 300K | 0.24 |
| Experiments, Haiku 5.5 (788 calls) | 32K / 350K / 237K | 0.07 |
| **Total** | | **2.38** |

**Cost reconciliation (an open question).** For the executor, the CLI's own `total_cost_usd` (`costBasis: list`)
is **$1.04**. My price table gives **$0.26** for exactly the same tokens:
- the token counts agree, and so do stage 1's costs ($0.062 both ways);
- the two diverge from the first `--resume` on;
- 20 of the 74 executor requests carried more than 200K tokens of context (maximum 287K), but a 2× long-context
  premium explains only about $0.08 of the gap.

So the conservative figure takes the CLI's executor number: **about $3.20 for a full run** ($1.04 + $2.07 + $0.07).
That is under the manual's projected $7.80, and under the $10 cap even at 3×.

## Quality caveats (read before quoting the timings)
Haiku took the shortest path through every skill:
- It skipped ARIS's evidence-gate helpers and ran 2 novelty searches, not the extended search.
- It did not review the stage-2 results.
- Paper writing skipped planning, figures, the improvement loop and the citation, proof and kill-argument audits.
- The paper cites one source.

The run is a fast, cheap, honest null result, not a polished paper. A real student who steers at the gates, adds a
harder corpus and runs the skipped audits will take longer and use more reviewer tokens. The reviewer is already 87% of the cost.

The executor logged its decisions with the date only, not the time the contract asked for.

## What the research found (for the record)
- Results:
  - The reimplemented ConsistencyGate (K=5, τ=0.7) removed all 16 incorrect facts at full recall on Haiku. Against
    admit-all, Δ contamination was −0.50, with 95% CI [−0.69, −0.34] and McNemar p = 3e−5.
  - The proposed polarity-mixed gate tied the baseline (Δ = 0) because the test was at the ceiling, and a harder
    near-miss corpus and screen did not break the baseline either.
- Reviewer verdict: 3/10, not ready. The claim audit fixed 5 mismatches and left 4 open items, including a
  bootstrap that ignores context clustering.

## Changes to the MP3 docs that follow from this
In `mp3-self-guided-manual-v3.md`:
- Part 0.1 now gives the measured cost (about $2.40–3.20), with the projection kept as an upper bound.
- The schedule notes about 1 h of agent time for a lite run with no steering.
