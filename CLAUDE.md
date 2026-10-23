# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A Markdown-only placement-preparation handbook (aptitude, core CS, Java, system design, dev tooling). There is no code, build, lint, or test step. "Correctness" here means technically accurate, interview-ready content that renders well on GitHub (KaTeX math via `$...$` / `$$...$$`).

## Routing: read the category rules before writing

The authoritative conventions live in `copilot-instructions.md` (root) and `.github/instructions/*.instructions.md`. These are not auto-loaded for Claude, so before answering or editing:

1. Classify the request into exactly one category (use the dominant one; ask one short question only if genuinely ambiguous). Do not duplicate content across categories.
2. Read the matching guidance file:

| Category | Folder | Guidance |
|---|---|---|
| Aptitude | `Aptitude/` | `.github/instructions/aptitude.instructions.md` |
| Core CS (OS, CN, DBMS/SQL) | `coreCS/` | `.github/instructions/core-cs.instructions.md` |
| Core Java | `coreJava/` | `.github/instructions/core-java.instructions.md` |
| System design | `systemArchitecture/` | `.github/instructions/system-architecture.instructions.md` |
| Dev tooling | `techStack/` | `techStack/README.md` (study-flow section) |

3. Use `Placement_Preparation_Handbook.md` (master syllabus, ~3.8k lines) and neighbouring notes as the source of truth for structure, terminology, and math style.

Every category shares the same explanation spine: **definition → mechanism → scenario → trade-off → common traps**.

## Per-section structure

- **Aptitude** — `Aptitude/<type>/<topic-slug>/` where `<type>` is `quantitative`, `logical-reasoning`, `verbal-ability`, or `data-interpretation`. Each topic has `README.md` (concept note: definition, formulae, methods, worked examples, shortcuts, traps, verification) and `questions.md` (numbered question log). Create a topic folder only when the first question for that topic arrives. **Append** new questions to `questions.md`; never replace earlier solutions. Each entry: original question, tested concept, approach, step-by-step working, final answer, verification, lesson/trap. Keep exact values until the last step. Update the parent `<type>/README.md` index when adding a topic.
- **coreCS** — one large file per subject (`Operating_Systems.md`, `Computer_Networks.md`, `DBMS_and_SQL.md`), each with a numbered table of contents and section headings tagged with the priority legend `🔴 MUST KNOW · 🟠 HIGH · 🟡 IMPORTANT · 🟢 GOOD TO KNOW`. Add focused sections in place (and update the TOC) rather than rewriting unrelated material.
- **coreJava** — topic files (`java_collections_framework.md`, `java_oops_checklist.md`). Include compilable Java examples; for collections compare ordering, duplicates, nulls, mutability, thread safety, complexity.
- **systemArchitecture** — start any design with functional/non-functional requirements, assumptions, and scale before components and trade-offs.
- **techStack** — each tool is a folder (e.g. `techStack/Git & Github/`, note the spaces and `&` — quote paths in shell commands and URL-encode as `Git%20%26%20Github` in Markdown links). Modules are numbered `NN-kebab-name/` each with a `README.md` and `checklist.md` (checkbox lists: Concepts / Commands & Practice / Interview Verification Questions). The tool's top-level `README.md` holds the curriculum index and rapid-revision sheet; `checklist.md` aggregates progress. When adding a module, update the curriculum index. Planned tools are listed in `techStack/README.md`.

## Mock tests (`mockTests/`)

Weekly timed OA papers written by Claude; `mockTests/README.md` defines tiers, patterns, and the scoreboard.

- **New paper:** create `mockTests/YYYY-MM/week-N-tierX/` (week of month) with `paper.md` and `answers.md`, following the previous paper's format and the tier pattern. Tier 1 = 17 MCQs (OS 3, CN 3, DBMS/SQL 3, OOP/Java 2, DSA 4, System design 2) + 2 coding + 1 SQL query (MySQL syntax, with sample tables and expected output), difficulty ramping per the README. Weight weak topics from the scoreboard. Add a scoreboard row. **Never write the answer key into the repo before the user submits**; verify coding samples with a reference solution in the scratchpad only.
- **User says "done":** read `answers.md` and the user's `Coding1.java` / `Coding2.java`, compile and run them (`javac`/`java`) against samples plus hidden edge cases, then write `review.md` in the paper folder: score + time, per-MCQ verdict with explanation (flag correct guesses), coding pass/fail + bugs + optimal Java solution with complexity, SQL query (from `answers.md`) run against sample + hidden data (`python3` sqlite3 or `psql`) with the optimal query, weak-topic learning notes that link to existing `coreCS/`, `coreJava/`, `techStack/` notes instead of duplicating them, practice problems, next-week focus. Fill in the scoreboard row.
- Coding answers and solutions are in Java.

## Repo conventions

- Root `README.md` has a section table linking every category; keep it in sync when adding a new section or tool.
- Commit messages use conventional-commit style scoped by section, e.g. `feat(techStack): ...`, `docs: ...`.
