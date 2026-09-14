# Permutations and Combinations

## Definition and scope

Permutations count arrangements where order matters. Combinations count selections where order does not matter. Restriction questions often become easier by counting all arrangements and subtracting invalid arrangements.

## Important concepts and assumptions

- Seven distinct letters have $7!$ unrestricted arrangements.
- Treating two letters as a single block reduces the number of objects by one.
- A block containing two distinct letters can be internally arranged in $2!$ ways.
- "Never come together" means the two letters must not be adjacent.

## Formulae and rules

$$
{}^nP_r = \frac{n!}{(n-r)!}
$$

$$
{}^nC_r = \frac{n!}{r!(n-r)!}
$$

For two specified letters that must not be adjacent:

$$
\text{Valid arrangements} = n! - 2(n-1)!
$$

The subtraction term counts the two internal orders of the adjacent pair.

## Standard solving method

1. Count all unrestricted arrangements.
2. Count arrangements where the restricted letters are together by treating them as one block.
3. Multiply by the number of internal arrangements within the block.
4. Subtract the invalid count from the total.

## Worked example

For A, B, C, D, E, F, G, the total arrangements are $7!$. If C and E are together, treat CE or EC as one block. There are six objects to arrange, and the pair has two internal orders, giving $2 \times 6!$ invalid arrangements. Therefore the valid count is $7! - 2 \times 6! = 3600$.

## Valid shortcuts and when not to use them

Use the complement method for a single adjacency restriction. For several restrictions, overlapping conditions may require inclusion-exclusion or a direct gap method.

## Common traps and edge cases

- A pair being together means adjacent, not merely appearing in the same arrangement.
- Do not forget the two internal orders of the block.
- Do not subtract only $6!$.
- If letters repeat, factorial counts must be adjusted for duplicate letters.

## Time-saving and verification techniques

The probability that two specified letters are adjacent in a random arrangement is $2/7$. Therefore the valid proportion is $1 - 2/7 = 5/7$, and $7! \times 5/7 = 3600$.
