# Mock Tests — Weekly OA Practice

One timed paper per week, modelled on real online assessments. Attempt it, submit, say **"done"**, and get a full review with optimal solutions and targeted learning notes.

---

## Workflow

1. **Ask for a new paper** → a folder `YYYY-MM/week-N-tierX/` is created with `paper.md` and `answers.md`.
2. **Attempt it timed** (no notes, no IDE autocomplete hints beyond what an OA allows).
3. **Submit:**
   - Fill the MCQ table and the SQL query block in `answers.md`.
   - Write coding answers as `Coding1.java` and `Coding2.java` in the same folder (`public class Coding1` / `Coding2`, read from stdin, print to stdout). Compile and test them yourself.
4. **Say "done"** → `review.md` is written in the same folder and the scoreboard below is updated.

The answer key is never written before you submit.

---

## Folder structure

```text
mockTests/
├── README.md
└── 2026-10/                 # month (YYYY-MM)
    └── week-4-tier1/        # week of the month + tier
        ├── paper.md         # questions
        ├── answers.md       # your MCQ answers + SQL query
        ├── Coding1.java     # your code
        ├── Coding2.java
        └── review.md        # review after "done"
```

---

## Tiers

| Tier | Status | Modelled on | Pattern |
|---|---|---|---|
| **Tier 1** | **Active** | TCS NQT, Infosys, HCL, Allegion | 17 MCQs (OS 3 · CN 3 · DBMS/SQL 3 · OOP/Java 2 · DSA 4 · System design 2) + 2 coding + 1 SQL query · 100 min |
| **Tier 2** | Later | Mphasis and smaller service companies | Aptitude-heavy: quant, logical reasoning, verbal + 1 coding |
| **Tier Trap** | Later | MAANG / product companies | 2–3 medium/hard DSA problems + trick MCQs |

Tier 1 difficulty ramps up: papers 1–2 easy → papers 3–4 easy-medium → medium (TCS NQT Advanced level). Weak topics from the scoreboard get extra weight in the next paper.

---

## Review format (`review.md`)

1. Score and time taken
2. MCQ-by-MCQ verdict with explanations (lucky guesses flagged)
3. Coding: your code run against sample + hidden edge cases, bugs found, optimal Java solution with complexity
4. SQL: your query run against sample + hidden data (NULLs, ties, empty groups), errors found, optimal query
5. Weak-topic learning notes, linked to the existing notes in [coreCS](../coreCS/README.md), [coreJava](../coreJava/README.md), and [techStack](../techStack/README.md), plus practice problems
6. Focus areas for next week

---

## Scoreboard

| Date | Paper | Tier | MCQ /17 | Coding /2 | SQL /1 | Time | Weak topics |
|---|---|---|---|---|---|---|---|
| 2026-10-23 | [week-4-tier1](2026-10/week-4-tier1/paper.md) | 1 | — | — | — | — | — |
