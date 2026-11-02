# Trial profile (generated 2026-11-02 05:01:51 EST)

Costs are API list-price equivalents of the measured tokens (the trial ran on the Max plan).

## Stages

| stage | wall min | exit | turns | executor result cost (CLI) |
|---|---:|---|---:|---:|
| 10_idea-discovery | 2.6 | success | 13 | 0.06 |
| 20_experiment-bridge | 22.2 | success | 5 | 0.31 |
| 30_auto-review-loop | 21.4 | success | 11 | 0.69 |
| 40_paper-writing | 2.3 | success | 7 | 0.85 |
| 50_paper-claim-audit | 7.9 | success | 9 | 1.04 |

## Tokens by stage, role and model

| stage | role | model | API msgs | input | output | cache read | cache write | list $ |
|---|---|---|---:|---:|---:|---:|---:|---:|
| 10_idea-discovery | executor-main | claude-haiku-5-5 | 10 | 20 | 16,221 | 598,215 | 69,132 | 0.028 |
| 10_idea-discovery | executor-subagent | claude-haiku-5-5 | 2 | 4 | 3,020 | 4,308 | 24,409 | 0.006 |
| 10_idea-discovery | reviewer | claude-sonnet-5-5 | 3 | 6 | 1,662 | 42,978 | 19,607 | 0.099 |
| 20_experiment-bridge | executor-main | claude-haiku-5-5 | 25 | 56 | 32,327 | 2,864,448 | 51,755 | 0.055 |
| 20_experiment-bridge | experiment | claude-haiku-5-5 | 500 | 1,000 | 14,877 | 206,528 | 167,370 | 0.043 |
| 20_experiment-bridge | reviewer | claude-sonnet-5-5 | 2 | 4 | 10,244 | 23,234 | 21,909 | 0.192 |
| 30_auto-review-loop | executor-main | claude-haiku-5-5 | 23 | 50 | 38,525 | 4,276,148 | 77,788 | 0.078 |
| 30_auto-review-loop | experiment | claude-haiku-5-5 | 288 | 576 | 17,363 | 143,680 | 69,452 | 0.024 |
| 30_auto-review-loop | reviewer | claude-sonnet-5-5 | 7 | 16 | 11,184 | 218,863 | 46,310 | 0.319 |
| 40_paper-writing | executor-main | claude-haiku-5-5 | 7 | 16 | 11,762 | 1,657,468 | 39,023 | 0.030 |
| 40_paper-writing | reviewer | claude-sonnet-5-5 | 8 | 16 | 9,871 | 246,872 | 42,406 | 0.293 |
| 50_paper-claim-audit | executor-main | claude-haiku-5-5 | 9 | 20 | 14,077 | 2,419,645 | 37,683 | 0.039 |
| 50_paper-claim-audit | reviewer | claude-sonnet-5-5 | 14 | 28 | 59,572 | 308,959 | 135,997 | 1.171 |
| **total** | | | | | | | | **2.38** |

## List-price cost by role

| role | model family | list $ |
|---|---|---:|
| reviewer | sonnet | 2.074 |
| executor-main | haiku | 0.230 |
| experiment | haiku | 0.067 |
| executor-subagent | haiku | 0.006 |

## Tool calls (time = tool_use to tool_result)

| role | tool | calls | total s | mean s | errors |
|---|---|---:|---:|---:|---:|
| executor-main | mcp__claude-review__review | 7 | 664 | 94.8 | 0 |
| executor-main | Bash | 54 | 305 | 5.6 | 3 |
| executor-main | Agent | 2 | 29 | 14.7 | 1 |
| executor-main | mcp__claude-review__review_reply | 1 | 23 | 23.4 | 0 |
| executor-subagent | WebFetch | 1 | 16 | 16.2 | 0 |
| executor-main | WebSearch | 2 | 13 | 6.3 | 0 |
| reviewer | Read | 68 | 5 | 0.1 | 0 |
| reviewer | Grep | 17 | 2 | 0.1 | 1 |
| executor-main | ToolSearch | 3 | 0 | 0.1 | 0 |
| reviewer | Glob | 1 | 0 | 0.1 | 0 |

## Slowest 15 tool calls

| stage | role | tool | s | detail |
|---|---|---|---:|---|
| 50_paper-claim-audit | executor-main | mcp__claude-review__review | 169 |  |
| 20_experiment-bridge | executor-main | Bash | 120 | Confirm main run alive and check progress |
| 30_auto-review-loop | executor-main | Bash | 119 | Pre-register and run the 48-call design screen |
| 50_paper-claim-audit | executor-main | mcp__claude-review__review | 118 |  |
| 50_paper-claim-audit | executor-main | mcp__claude-review__review | 111 |  |
| 20_experiment-bridge | executor-main | mcp__claude-review__review | 96 |  |
| 30_auto-review-loop | executor-main | mcp__claude-review__review | 80 |  |
| 40_paper-writing | executor-main | mcp__claude-review__review | 74 |  |
| 20_experiment-bridge | executor-main | Bash | 55 | Run debug pass with 20-call budget |
| 10_idea-discovery | executor-main | Agent | 29 | Fetch ConsistencyGate paper method |
| 30_auto-review-loop | executor-main | mcp__claude-review__review_reply | 23 |  |
| 10_idea-discovery | executor-subagent | WebFetch | 16 |  |
| 10_idea-discovery | executor-main | mcp__claude-review__review | 16 |  |
| 10_idea-discovery | executor-main | WebSearch | 6 |  |
| 10_idea-discovery | executor-main | WebSearch | 6 |  |

## Skills invoked


## Reviewer bridge calls

- calls: 8; total 687 s

## Experiment calls (claude-haiku-5-5 via claude -p)

- calls 788, ok 788, status {'ok': 788}
- wall per call p50 2.17 s, p95 5.111 s
- tokens {'input_tokens': 1576, 'cache_creation_input_tokens': 236822, 'cache_read_input_tokens': 350208, 'output_tokens': 32240}
- list price $0.067 ($0.00009 per call)
- by tag prefix: {'arm=C': 180, 'arm=B': 160, 'arm=D': 160, 'v2': 240, 'screen3': 48}
