---
description: "Use when classifying, solving, documenting, or organizing aptitude questions, including quantitative aptitude, logical reasoning, verbal ability, and data interpretation."
applyTo: "Aptitude/**/*.md"
---
# Aptitude Instructions

## Classification and folders

Classify each aptitude question into one of these types:

- `quantitative`
- `logical-reasoning`
- `verbal-ability`
- `data-interpretation`

For a new topic, create a normalized topic folder inside the type folder. Example: an HCF and LCM question belongs in `Aptitude/quantitative/hcf-lcm/`.

Do not create topic folders speculatively. Create them when the first question for that topic is asked.

## Topic note

Create `<topic-slug>/README.md` with:

1. Definition and scope
2. Important concepts and assumptions
3. Formulae, identities, or rules
4. Standard solving methods
5. Worked examples
6. Valid shortcuts and when not to use them
7. Common traps and edge cases
8. Time-saving or verification techniques

Use KaTeX-compatible math consistent with `Placement_Preparation_Handbook.md`.

## Question log

Create or update `<topic-slug>/questions.md`. Append each question as a new dated or numbered section. Never erase previous solutions.

Every entry should include:

- Original question
- Tested concept
- Approach
- Step-by-step calculation or reasoning
- Final answer
- Quick verification or alternative method when useful
- Mistake, trap, or lesson to remember

Use exact values until the final step unless approximation is explicitly appropriate. State units and assumptions, and identify ambiguity instead of inventing missing information.
