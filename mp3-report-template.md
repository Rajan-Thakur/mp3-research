# MP3 Final Report: [paper title], [your name]

Copy this file to `REPORT.md` in your project root and fill every section. Keep the headings. Everything you claim must point to a file in your repository (`path:line` or a results key); the grader reads the files, not the prose. 1,200–2,000 words plus tables. There is no presentation or video: this report is a quarter of the raw score (rubric R1–R4), and sections 5b and 5c are the design and evidence questions an oral defense would have asked. If you only completed run A, say so in §2 and §7 and compare run A to the instructor's run B in §4. A sample filled in from the instructor's two trial runs is in `examples/consistencygate/SAMPLE_REPORT.md`.

## 1. The paper and why I chose it (C1, eligibility)
- Base paper: [title], arXiv:[ID], v1 [date], [pages] pages, code: [URL or "none, reimplemented"].
- Class topic it belongs to: [token maxing / context rot / memory in files / skills / hooks / loop engineering / self-consistency / benchmarks].
- The mechanism in one sentence: …
- Why it passed the paper test (`mp3-paper-search-prompt.md` constraints a–e), and what the second-check critique said: … (`notes/paper-choice.md`)
- The gap I chose and the evidence for it: … (`notes/gap.md:…`)

## 2. The two runs (T1, T4)
| | Run A: unguided | Run B: guided |
|---|---|---|
| Command and session ID | `notes/aris-commands.md:…` | `notes/aris-commands.md:…` |
| Agent wall time | | |
| Experiment calls / reviewer calls | | |
| API spend before → after (console) | | |
| Gates answered | 0 | [n] |
| Outputs | `runs/A/…` | project root, tag `mp3-ai-draft` |

## 3. How I steered run B (C3)
List your three assessed decisions here as `### Decision N` blocks (or point to `notes/steering-log.md`). Each needs: raw pointer `path:line` into the exported transcript, the agent's proposal quoted, your question, `DECISION: ACCEPT | REJECT | MODIFY — reason`, and the call budget you allowed.

### Decision 1
### Decision 2
### Decision 3

One paragraph: what you changed that the unguided run did not do, and what it cost (calls, reviewer rounds, time).

## 4. Judge scores for run A and run B, and why they differ
Score both runs' outputs with the rubric's judge prompt (manual Part 9.2), items F1, F3, T3, T5, D1, D2, C1, C2, C4. Save the JSON as `notes/judge-A.json` and `notes/judge-B.json`.

| Item | Run A | Run B | Where the difference comes from (file) |
|---|---:|---:|---|
| F1 Baseline | | | |
| F3 Controlled comparison | | | |
| T3 Stage handoffs | | | |
| T5 Numerical provenance | | | |
| D1 Reference identity | | | |
| D2 Citation support | | | |
| C1 Gap and novelty | | | |
| C2 Method contribution | | | |
| C4 Critical appraisal | | | |
| Sum | | | |

Two or three paragraphs: which decisions of yours produced which differences; where B is not better and why; what the ceiling check showed.

## 5. What my human review found in the AI-written draft (D3, X1, X2)
From `notes/human-revision.md`, `notes/reference-check.md`, `notes/claims.md`, `notes/citation-check.md`:
- Numbers that did not trace, and what you did: …
- References that did not resolve or did not say what the paper claimed (hallucinated or over-stated), and what you did: …
- Claims wider than the evidence, and how you narrowed them: …
- AI-slop passages you removed or rewrote (generic openers, unsupported superlatives, empty transitions), with before/after examples: …
- Checks that found nothing wrong, and what you checked: …

## 5b. Design explanations, written (R2)
Three answers, each naming a concrete choice, the trade-off, and the file that shows it:
- Why this method change: …
- Why these controls and this budget: …
- What ARIS and the reviewer did, and why I kept or changed it: …

## 5c. Evidence questions, answered in writing (R3)
- Q1. A decision where I evaluated the agent's advice (name one Decision block above, quote the evidence with `path:line`, explain its bearing): …
- Q2. The weakest point of my result and the test that would settle it: …
- Q3. One number from my abstract, traced to its raw rows and the formula: value → `results/…` → aggregate key/formula: …

## 6. Result and limitations (C2, C4)
- The result in two sentences, with counts beside every rate and the paired interval with its discordant count: …
- What the test could and could not show: …
- Two alternatives I did not take, and why; two threats to validity and their direction; the next experiment and what would change my conclusion: …

## 7. Budget and integrity
- Final API spend (console): $…; Pro limit stops: [n]; ledger: `notes/usage-ledger.csv`.
- Generative AI Use Statement and backends table: `README.md:…`.
- Anything that went wrong (crash, cap, stop) and what you did: `notes/recovery.md`.
