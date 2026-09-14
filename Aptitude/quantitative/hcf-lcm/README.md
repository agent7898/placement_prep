# HCF and LCM

## Definition and scope

The highest common factor (HCF), also called the greatest common divisor, is the greatest positive integer that divides each number. The least common multiple (LCM) is the smallest positive integer divisible by each number.

Many aptitude questions express two numbers as a common HCF multiplied by separate factors:

$$
N_1 = H \times a, \qquad N_2 = H \times b
$$

where $H$ is the HCF and $a$ and $b$ are intended to be coprime co-factors.

## Important concepts and assumptions

- Prime factorization provides both HCF and LCM directly.
- Euclid's algorithm computes the HCF efficiently.
- For two positive integers $m$ and $n$:

$$
\operatorname{HCF}(m,n) \times \operatorname{LCM}(m,n) = m \times n
$$

- If $m = H a$ and $n = H b$ with $\gcd(a,b)=1$, then:

$$
\operatorname{LCM}(m,n) = H a b
$$

## Formulae and rules

For prime factorizations, use the minimum exponent for HCF and the maximum exponent for LCM.

If the HCF is $H$ and the two coprime co-factors are $a$ and $b$:

$$
\text{smaller number} = H \times \min(a,b)
$$

$$
\text{larger number} = H \times \max(a,b)
$$

## Standard solving methods

1. Identify the common HCF.
2. Interpret the remaining values as co-factors of the two numbers.
3. Multiply each co-factor by the HCF.
4. Select the larger product.
5. Verify the actual HCF if the co-factors are stated to be coprime.

## Worked example

If $H=30$ and the intended co-factors are 10 and 12, the two intended numbers are:

$$
30 \times 10 = 300, \qquad 30 \times 12 = 360
$$

Therefore, the larger intended number is $360$.

However:

$$
\gcd(300,360)=60
$$

So 10 and 12 are not coprime co-factors, and the wording is mathematically inconsistent if it requires the actual HCF to be exactly 30. In standard aptitude-question convention, the expected answer is still obtained by multiplying the larger stated factor by the HCF.

## Valid shortcuts and when not to use them

When a question clearly gives the HCF and the two co-factors, multiply the larger co-factor by the HCF. Do not use this shortcut without checking whether the problem's wording means co-factors or merely factors of the LCM.

## Common traps and edge cases

- HCF and LCM are not interchangeable.
- The two co-factors must be coprime if the displayed HCF is to remain exactly $H$.
- If the wording is inconsistent, state the intended exam interpretation and the strict mathematical issue.
- The product relationship applies to positive integers.

## Time-saving and verification techniques

Use prime factors or Euclid's algorithm to verify the HCF. For a claimed pair of numbers, calculate their actual gcd before accepting the result.
