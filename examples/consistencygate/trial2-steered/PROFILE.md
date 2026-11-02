# Trial profile (generated 2026-11-02 09:47:42 EST)

Costs are API list-price equivalents of the measured tokens (the trial ran on the Max plan).

## Stages

| stage | wall min | exit | turns | executor result cost (CLI) |
|---|---:|---|---:|---:|
| 10_idea-discovery_s0 | 1.1 | success | 15 | 0.05 |
| 10_idea-discovery_s0_student | 0.3 | success | 2 | 0.07 |
| 10_idea-discovery_s1 | 1.8 | success | 10 | 0.18 |
| 10_idea-discovery_s1_student | 0.3 | success | 2 | 0.11 |
| 10_idea-discovery_s2 | 2.5 | success | 14 | 0.35 |
| 10_idea-discovery_s2_student | 0.3 | success | 4 | 0.15 |
| 10_idea-discovery_s3 | 10.0 | success | 8 | 0.47 |
| 10_idea-discovery_s3_student | 0.3 | success | 3 | 0.18 |
| 10_idea-discovery_s4 | 3.8 | success | 12 | 0.63 |
| 20_experiment-bridge_s0 | 4.4 | success | 12 | 0.86 |
| 20_experiment-bridge_s0_student | 0.3 | success | 2 | 0.22 |
| 20_experiment-bridge_s1 | 2.6 | success | 6 | 0.97 |
| 20_experiment-bridge_s1_student | 0.3 | success | 6 | 0.27 |
| 20_experiment-bridge_s2 | 8.9 | success | 7 | 1.10 |
| 30_auto-review-loop_s0 | 3.3 | success | 11 | 1.35 |
| 30_auto-review-loop_s0_student | 0.3 | success | 4 | 0.33 |
| 30_auto-review-loop_s1 | 3.4 | success | 12 | 1.65 |
| 30_auto-review-loop_s1_student | 0.3 | success | 2 | 0.36 |
| 30_auto-review-loop_s2 | 0.8 | success | 5 | 1.77 |
| 40_paper-writing_s0 | 4.5 | success | 12 | 2.11 |
| 40_paper-writing_s0_student | 0.3 | success | 4 | 0.43 |
| 40_paper-writing_s1 | 6.5 | success | 22 | 2.76 |
| 40_paper-writing_s1_student | 0.3 | success | 6 | 0.49 |
| 40_paper-writing_s2 | 7.4 | success | 33 | 3.79 |
| 40_paper-writing_s2_student | 0.4 | success | 8 | 0.58 |
| 40_paper-writing_s3 | 0.9 | success | 10 | 4.09 |
| 50_paper-claim-audit_s0 | 5.3 | success | 21 | 4.30 |
| 50_paper-claim-audit_s0_student | 0.3 | success | 2 | 0.62 |
| 50_paper-claim-audit_s1 | 0.9 | success | 12 | 4.43 |

## Tokens by stage, role and model

| stage | role | model | API msgs | input | output | cache read | cache write | list $ |
|---|---|---|---:|---:|---:|---:|---:|---:|
| 10_idea-discovery_s0 | executor-main | claude-haiku-5-5 | 11 | 22 | 9,548 | 836,651 | 91,883 | 0.032 |
| 10_idea-discovery_s0 | student-sim | claude-sonnet-5-5 | 1 | 2 | 118 | 2,604 | 12,089 | 0.050 |
| 10_idea-discovery_s0_student | student-sim | claude-sonnet-5-5 | 1 | 2 | 962 | 14,693 | 2,095 | 0.019 |
| 10_idea-discovery_s1 | executor-main | claude-haiku-5-5 | 8 | 16 | 11,048 | 896,429 | 18,870 | 0.018 |
| 10_idea-discovery_s1 | student-sim | claude-sonnet-5-5 | 1 | 4 | 143 | 16,788 | 2,072 | 0.011 |
| 10_idea-discovery_s1_student | student-sim | claude-sonnet-5-5 | 1 | 2 | 1,195 | 18,860 | 3,570 | 0.028 |
| 10_idea-discovery_s2 | executor-main | claude-haiku-5-5 | 14 | 28 | 18,603 | 1,878,098 | 26,617 | 0.033 |
| 10_idea-discovery_s2 | experiment | claude-haiku-5-5 | 20 | 40 | 1,411 | 0 | 15,509 | 0.004 |
| 10_idea-discovery_s2 | student-sim | claude-sonnet-5-5 | 2 | 6 | 209 | 47,014 | 2,412 | 0.016 |
| 10_idea-discovery_s2_student | student-sim | claude-sonnet-5-5 | 1 | 2 | 1,182 | 24,842 | 1,918 | 0.022 |
| 10_idea-discovery_s3 | executor-main | claude-haiku-5-5 | 8 | 16 | 14,161 | 1,263,211 | 17,671 | 0.023 |
| 10_idea-discovery_s3 | experiment | claude-haiku-5-5 | 240 | 480 | 16,743 | 162,722 | 23,246 | 0.015 |
| 10_idea-discovery_s3 | student-sim | claude-sonnet-5-5 | 1 | 4 | 159 | 26,760 | 2,168 | 0.013 |
| 10_idea-discovery_s3_student | student-sim | claude-sonnet-5-5 | 1 | 2 | 1,406 | 28,928 | 1,356 | 0.022 |
| 10_idea-discovery_s4 | executor-main | claude-haiku-5-5 | 11 | 22 | 16,380 | 1,947,526 | 23,223 | 0.032 |
| 10_idea-discovery_s4 | reviewer | claude-sonnet-5-5 | 7 | 14 | 17,193 | 312,397 | 71,281 | 0.488 |
| 20_experiment-bridge_s0 | executor-main | claude-haiku-5-5 | 12 | 26 | 26,037 | 2,572,735 | 40,629 | 0.047 |
| 20_experiment-bridge_s0 | reviewer | claude-sonnet-5-5 | 5 | 10 | 16,335 | 136,286 | 43,121 | 0.349 |
| 20_experiment-bridge_s0 | student-sim | claude-sonnet-5-5 | 1 | 4 | 117 | 30,284 | 2,289 | 0.013 |
| 20_experiment-bridge_s0_student | executor-main | claude-haiku-5-5 | 1 | 2 | 236 | 230,407 | 2,116 | 0.003 |
| 20_experiment-bridge_s0_student | student-sim | claude-sonnet-5-5 | 1 | 2 | 1,329 | 32,573 | 3,017 | 0.029 |
| 20_experiment-bridge_s1 | executor-main | claude-haiku-5-5 | 5 | 10 | 9,875 | 1,195,867 | 13,409 | 0.020 |
| 20_experiment-bridge_s1 | experiment | claude-haiku-5-5 | 18 | 36 | 801 | 0 | 14,393 | 0.003 |
| 20_experiment-bridge_s1 | student-sim | claude-sonnet-5-5 | 3 | 8 | 336 | 110,812 | 2,394 | 0.024 |
| 20_experiment-bridge_s1_student | student-sim | claude-sonnet-5-5 | 1 | 2 | 1,271 | 37,984 | 1,489 | 0.022 |
| 20_experiment-bridge_s2 | executor-main | claude-haiku-5-5 | 7 | 14 | 10,308 | 1,766,496 | 13,191 | 0.025 |
| 20_experiment-bridge_s2 | experiment | claude-haiku-5-5 | 96 | 192 | 6,274 | 67,277 | 9,611 | 0.006 |
| 30_auto-review-loop_s0 | executor-main | claude-haiku-5-5 | 11 | 24 | 14,779 | 3,263,001 | 52,515 | 0.051 |
| 30_auto-review-loop_s0 | reviewer | claude-sonnet-5-5 | 5 | 10 | 13,615 | 158,857 | 74,245 | 0.449 |
| 30_auto-review-loop_s0 | student-sim | claude-sonnet-5-5 | 2 | 6 | 503 | 81,281 | 6,881 | 0.041 |
| 30_auto-review-loop_s0_student | student-sim | claude-sonnet-5-5 | 1 | 2 | 988 | 46,354 | 841 | 0.018 |
| 30_auto-review-loop_s1 | executor-main | claude-haiku-5-5 | 12 | 24 | 25,317 | 3,949,651 | 36,430 | 0.059 |
| 30_auto-review-loop_s1 | reviewer | claude-sonnet-5-5 | 3 | 8 | 7,254 | 256,105 | 18,728 | 0.173 |
| 30_auto-review-loop_s1 | student-sim | claude-sonnet-5-5 | 1 | 4 | 307 | 47,195 | 1,801 | 0.015 |
| 30_auto-review-loop_s1_student | student-sim | claude-sonnet-5-5 | 1 | 2 | 988 | 48,996 | 710 | 0.018 |
| 30_auto-review-loop_s2 | executor-main | claude-haiku-5-5 | 5 | 10 | 7,410 | 1,762,495 | 9,531 | 0.023 |
| 40_paper-writing_s0 | executor-main | claude-haiku-5-5 | 11 | 22 | 24,132 | 4,391,667 | 67,939 | 0.070 |
| 40_paper-writing_s0 | reviewer | claude-sonnet-5-5 | 5 | 10 | 15,710 | 134,818 | 44,802 | 0.350 |
| 40_paper-writing_s0 | student-sim | claude-sonnet-5-5 | 2 | 6 | 217 | 101,143 | 1,923 | 0.020 |
| 40_paper-writing_s0_student | executor-main | claude-haiku-5-5 | 1 | 2 | 496 | 425,538 | 1,971 | 0.005 |
| 40_paper-writing_s0_student | student-sim | claude-sonnet-5-5 | 1 | 2 | 1,284 | 51,629 | 7,298 | 0.047 |
| 40_paper-writing_s1 | executor-main | claude-haiku-5-5 | 20 | 40 | 41,476 | 9,227,743 | 59,647 | 0.125 |
| 40_paper-writing_s1 | reviewer | claude-sonnet-5-5 | 6 | 12 | 16,919 | 193,395 | 52,351 | 0.398 |
| 40_paper-writing_s1 | student-sim | claude-sonnet-5-5 | 2 | 6 | 408 | 119,925 | 2,287 | 0.025 |
| 40_paper-writing_s1_student | student-sim | claude-sonnet-5-5 | 2 | 4 | 1,680 | 122,984 | 2,119 | 0.038 |
| 40_paper-writing_s2 | executor-main | claude-haiku-5-5 | 33 | 66 | 47,741 | 16,934,585 | 59,643 | 0.205 |
| 40_paper-writing_s2 | reviewer | claude-sonnet-5-5 | 2 | 4 | 19,935 | 23,493 | 40,845 | 0.365 |
| 40_paper-writing_s2 | student-sim | claude-sonnet-5-5 | 1 | 4 | 163 | 63,333 | 2,002 | 0.016 |
| 40_paper-writing_s2_student | student-sim | claude-sonnet-5-5 | 5 | 10 | 2,156 | 334,279 | 3,412 | 0.069 |
| 40_paper-writing_s3 | executor-main | claude-haiku-5-5 | 10 | 20 | 7,644 | 5,516,823 | 10,155 | 0.061 |
| 50_paper-claim-audit_s0 | executor-main | claude-haiku-5-5 | 14 | 28 | 28,294 | 2,133,083 | 102,045 | 0.056 |
| 50_paper-claim-audit_s0 | reviewer | claude-sonnet-5-5 | 6 | 12 | 18,280 | 207,424 | 68,627 | 0.478 |
| 50_paper-claim-audit_s0 | student-sim | claude-sonnet-5-5 | 1 | 4 | 304 | 68,747 | 2,139 | 0.018 |
| 50_paper-claim-audit_s0_student | executor-main | claude-haiku-5-5 | 1 | 2 | 635 | 106,361 | 2,061 | 0.002 |
| 50_paper-claim-audit_s0_student | student-sim | claude-sonnet-5-5 | 1 | 2 | 810 | 70,886 | 1,239 | 0.020 |
| 50_paper-claim-audit_s1 | executor-main | claude-haiku-5-5 | 9 | 18 | 8,505 | 1,192,810 | 40,711 | 0.024 |
| **total** | | | | | | | | **4.61** |

## List-price cost by role

| role | model family | list $ |
|---|---|---:|
| reviewer | sonnet | 3.051 |
| executor-main | haiku | 0.914 |
| student-sim | sonnet | 0.616 |
| experiment | haiku | 0.028 |

## Tool calls (time = tool_use to tool_result)

| role | tool | calls | total s | mean s | errors |
|---|---|---:|---:|---:|---:|
| executor-main | Bash | 156 | 1202 | 7.7 | 3 |
| executor-main | mcp__claude-review__review | 7 | 885 | 126.4 | 0 |
| executor-main | mcp__claude-review__review_reply | 1 | 58 | 57.5 | 0 |
| executor-main | WebSearch | 3 | 30 | 9.8 | 0 |
| reviewer | Read | 82 | 8 | 0.1 | 0 |
| reviewer | Grep | 29 | 3 | 0.1 | 0 |
| executor-main | Write | 12 | 1 | 0.1 | 0 |
| student-sim | Glob | 12 | 1 | 0.1 | 0 |
| student-sim | Grep | 9 | 1 | 0.1 | 0 |
| student-sim | Read | 11 | 1 | 0.1 | 0 |
| executor-main | Edit | 8 | 1 | 0.1 | 0 |
| executor-main | Read | 6 | 1 | 0.1 | 0 |
| executor-main | Skill | 4 | 0 | 0.1 | 0 |
| reviewer | Glob | 4 | 0 | 0.1 | 0 |
| executor-main | ToolSearch | 3 | 0 | 0.1 | 0 |
| student-sim | Bash | 1 | 0 | 0.0 | 1 |
| reviewer | Bash | 1 | 0 | 0.0 | 1 |
| reviewer | Write | 1 | 0 | 0.0 | 1 |

## Slowest 15 tool calls

| stage | role | tool | s | detail |
|---|---|---|---:|---|
| 10_idea-discovery_s3 | executor-main | Bash | 521 | Poll main run progress until draws complete or timeout |
| 20_experiment-bridge_s2 | executor-main | Bash | 473 | Run the 96-call slice in background and poll to completion |
| 50_paper-claim-audit_s0 | executor-main | mcp__claude-review__review | 137 |  |
| 20_experiment-bridge_s0 | executor-main | mcp__claude-review__review | 134 |  |
| 40_paper-writing_s2 | executor-main | mcp__claude-review__review | 134 |  |
| 10_idea-discovery_s4 | executor-main | mcp__claude-review__review | 132 |  |
| 40_paper-writing_s1 | executor-main | mcp__claude-review__review | 125 |  |
| 40_paper-writing_s0 | executor-main | mcp__claude-review__review | 114 |  |
| 30_auto-review-loop_s0 | executor-main | mcp__claude-review__review | 108 |  |
| 20_experiment-bridge_s1 | executor-main | Bash | 90 | Simplify criterion 4; run approved D1 debug (18 calls) |
| 30_auto-review-loop_s1 | executor-main | mcp__claude-review__review_reply | 58 |  |
| 10_idea-discovery_s2 | executor-main | Bash | 37 | Run the 20-call debug scoring pass with haiku |
| 10_idea-discovery_s1 | executor-main | Bash | 20 | Fetch six candidate arXiv abstract pages to verify |
| 10_idea-discovery_s1 | executor-main | WebSearch | 14 |  |
| 10_idea-discovery_s1 | executor-main | WebSearch | 10 |  |

## Skills invoked

- 10_idea-discovery_s0 `browse` (0 s)
- 40_paper-writing_s0 `paper-plan` (0 s)
- 40_paper-writing_s1 `handoff` (0 s)
- 50_paper-claim-audit_s1 `browse` (0 s)

## Reviewer bridge calls

- calls: 8; total 942 s

## Experiment calls (claude-haiku-5-5 via claude -p)

- calls 374, ok 374, status {'ok': 374}
- wall per call p50 2.48 s, p95 5.37 s
- tokens {'input_tokens': 748, 'cache_creation_input_tokens': 62759, 'cache_read_input_tokens': 229999, 'output_tokens': 25229}
- list price $0.028 ($0.00007 per call)
- by tag prefix: {'debug': 20, 'main': 240, 'v2': 114}
