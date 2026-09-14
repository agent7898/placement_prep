# Averages

## Definition and scope

An average is the total of all values divided by the number of values. In placement questions, averages often involve additions, removals, replacements, or a changing total count.

## Important concepts and assumptions

- `Total = Average x Count`
- `Average = Total / Count`
- A percentage such as 30% of 60 represents a count when the context is a number of games, questions, or items.
- When new values are added, both the total and the count may change.

## Formulae and rules

$$
\text{Average} = \frac{\text{Total}}{\text{Count}}
$$

$$
\text{Total} = \text{Average} \times \text{Count}
$$

For adding a value $x$ to a group with total $T$ and count $n$:

$$
\text{New Average} = \frac{T + x}{n + 1}
$$

For a target average after adding $k$ values with combined total $S$:

$$
\frac{T + S}{n + k} = \text{Target Average}
$$

## Standard solving method

1. Convert the initial average or percentage into the initial total.
2. Let the unknown added quantity be a variable.
3. Express the new total and new count.
4. Set the new average equal to the target average.
5. Solve and verify that the result is a valid whole number when counting discrete objects.

## Worked example

A team won 30% of its first 60 games and then reached a 50% win average after winning $x$ consecutive games.

Initial wins:

$$
0.30 \times 60 = 18
$$

After the streak, wins are $18 + x$ and games are $60 + x$:

$$
\frac{18 + x}{60 + x} = 0.50
$$

Solving gives $x = 24$.

## Valid shortcuts and when not to use them

For a target average of 50%, compare the initial deficit to 50%. The team had 18 wins but needed 30 wins for a 50% average over 60 games, so it was short by 12 wins. Each additional win contributes a net gain of half a win toward the 50% target because one win raises both the numerator and denominator. Therefore, $12 \div 0.5 = 24$ wins.

Use this shortcut only when every added game is a win and the target average is clearly defined over the enlarged total.

## Common traps and edge cases

- Do not calculate 50% of the original 60 games and stop; the denominator increases during the streak.
- Do not add the streak only to the number of wins while keeping games fixed at 60.
- A percentage of a count must produce a valid count in the given context.
- If the new games include losses or draws, use their actual contribution rather than assuming every added game is a win.

## Time-saving and verification techniques

After solving, substitute the value back into both totals. For $x = 24$, the team has 42 wins in 84 games, and $42/84 = 50\%$.
