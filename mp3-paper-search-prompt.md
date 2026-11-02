# MP3 paper-search prompt

Paste the block below into Claude Code (it needs web access: `WebSearch`/`WebFetch`, or `curl`) or into claude.ai. Replace `[TOPIC]` with one technique from class. Run it once per topic. Then verify every candidate yourself on its arXiv abstract page before you choose; the prompt finds, you decide (manual §2.1–2.2).

```text
I am choosing a base paper for IPHS 400 Mini-Project 3 (Kenyon College, Fall 2026, due 2026-11-08).
Topic: [TOPIC]
(one of: token maxing / token budgeting; context rot and context management; externalizing memory to files;
agent skills; hooks; loop engineering; self-consistency and verification; small benchmarks and harness evaluation)

Search arxiv.org and return up to 5 candidate papers that satisfy ALL of these constraints. Reject anything
that fails one; do not relax a constraint to fill the table.

(a) RECENT: first version (v1) posted on or after 2026-05-02 (no more than 6 months before 2026-11-02).
    Check the Submission history on the abstract page; a later revision does not count.
(b) SHORT, SIMPLE, FOCUSED METHODS PAPER: it proposes and evaluates ONE mechanism (a prompting, decoding,
    verification, routing, retrieval, memory or context-handling technique that runs on top of an API model)
    that can be stated in one sentence and changed in one place. About 6-9 pages in total; over 12 pages
    before references is out. Not a theory, survey, position, benchmark-only or measurement paper; not a
    system with many interacting components; nothing that trains or fine-tunes a model or needs a GPU.
(c) CHEAP TO RE-RUN: the evaluation is a small, public, text-only task with an automatic score (exact match,
    unit test, checker), or a synthetic task that can be generated and scored by script. No new data
    collection, no web-scale crawl, no large dataset download, no human labels. Baseline plus one change must
    fit in about 300-600 calls to claude-haiku-5-5, so one item costs at most a handful of calls. This must
    fit a student API budget with a $20 hard cap, of which about $10 is planned.
(d) PREFERRED: public code (GitHub) that runs with uv and plain Python; the paper reports its own baseline
    numbers on the small task so a scoped reproduction can be compared.
(e) EXCLUDED: ConsistencyGate (arXiv:2607.22962), and any paper whose main result needs a frontier model,
    a multi-step agent benchmark (SWE-bench, web or terminal agents) or Docker environments.

How to search: use the arXiv advanced search with date-from 2026-05-02 on the submitted date, the topic words
in the title or abstract, and computer-science classification; read the abstract page of each hit. Make at
most one arXiv request every 3 seconds. Say "unknown" for anything you could not verify; never invent an
ID, a date, a page count or a code link.

Return one table, one row per candidate, columns:
  arXiv ID | title | v1 date | pages (total) | the mechanism in one sentence | task and metric used |
  calls per item (estimate) | code link or "none" | risks (one line: what could make this fail (a)-(e))
Then one line per candidate: "fits / doubtful / fails" with the constraint letter that decides it.
Rank by fit, then by simplicity. Do not write anything else.
```

What to do with the table: open each abstract page yourself and confirm the v1 date and the page count; open the code link and check the licence; then answer the five questions in `notes/paper-choice.md`. If the best candidate is "doubtful", ask the instructor before you invest time.

## Second check: have Claude critique your pick before you commit

Once you have a favourite, run this in Claude Code with the paper's abstract page open (or its PDF in the project). It argues *against* the paper; if it cannot find a failing constraint, you are on solid ground. Save the reply in `notes/paper-choice.md`.

```text
I intend to use this paper as my MP3 base paper: arXiv:[ID], "[title]". Read its abstract page and, if
available, the PDF. Check it against every constraint in mp3-paper-search-prompt.md, one at a time:
(a) v1 date on or after 2026-05-02; (b) one mechanism, stated in a sentence, changeable in one place,
6-9 pages, not theory/survey/position/benchmark-only, no training or GPU; (c) re-runnable with
claude-haiku-5-5 in 300-600 calls on its own small public task or a scripted synthetic one, no new data
collection; (d) code available and runnable with uv; (e) not excluded.
For each constraint answer PASS / FAIL / UNKNOWN with the page or section that decides it. Then:
1. State the mechanism in one sentence and name the one place a student could change it.
2. Estimate calls per item for baseline + one change, and the total for 30 items per arm.
3. Name the single biggest risk that this project does not finish in a week on a $20 API cap.
4. Give a verdict: USE / USE WITH THIS SCOPE CUT / PICK ANOTHER, with the reason.
Say UNKNOWN rather than guessing. Do not propose the improvement yet.
```
