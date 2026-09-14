# Permutations and Combinations Questions

## 1. Arrangements when two letters cannot be together

### Original question

If C and E never come together, how many ways can letters A, B, C, D, E, F, G be arranged?

### Tested concept

Counting permutations with a forbidden adjacency using the complement method.

### Approach

Count all arrangements, then subtract arrangements in which C and E are adjacent. Treat C and E as one block for the invalid count, and account for the two possible internal orders: CE and EC.

### Step-by-step calculation

Total arrangements of seven distinct letters:

$$
7! = 5040
$$

For arrangements where C and E are together, treat them as one block. There are now six objects to arrange:

$$
6!
$$

The block can be ordered as CE or EC, so invalid arrangements are:

$$
2 \times 6! = 2 \times 720 = 1440
$$

Valid arrangements:

$$
7! - 2 \times 6! = 5040 - 1440 = 3600
$$

### Final answer

$$
\boxed{3600}
$$

There are **3600 ways**.

### Verification

The probability that two specified letters are adjacent in an arrangement of seven distinct letters is $2/7$. Therefore the non-adjacent proportion is $5/7$:

$$
7! \times \frac{5}{7} = 3600
$$

### Mistake or trap to remember

Do not subtract only $6!$. The adjacent pair has two internal orders: CE and EC.
