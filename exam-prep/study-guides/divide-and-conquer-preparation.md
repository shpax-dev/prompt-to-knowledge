# Divide-and-Conquer Preparation

This guide covers Lectures 4–6. Every divide-and-conquer question comes down to the lecture's standard questions:

1. How many recursive calls are there?
2. What size is each call?
3. What does dividing and combining cost per call?
4. What recurrence results, and what does it solve to?

## Work in this order

1. **Write the algorithm** with an explicit base case and combine step. Model it on [[review/divide-and-conquer/merge-sort-and-the-paradigm|merge sort]].
2. **Prove correctness by strong induction:** soundness, meaning the combine step is right when the subresults are right, and completeness, meaning every call shrinks toward the base case.
3. **Solve the recurrence** with the [[review/divide-and-conquer/solving-recurrences|Master theorem]]. Use Akra–Bazzi when the split is unequal.
4. **Check against a lower bound.** If you have matched the [[review/divide-and-conquer/lower-bounds|decision-tree bound]], you are done.

## Pattern recognition

| Clue in the problem | Technique |
| :--- | :--- |
| Count pairs that are out of order | [[review/divide-and-conquer/counting-inversions\|Merge-and-count]] |
| Cut the number of multiplications | [[review/divide-and-conquer/fast-multiplication\|Karatsuba / Strassen]] |
| Geometry with a dividing line | [[review/divide-and-conquer/closest-pair-of-points\|Strip argument]] |
| k-th order statistic | [[review/divide-and-conquer/selection-and-median-of-medians\|Quickselect / median of medians]] |
| Minimize the maximum, with monotone feasibility | [[review/divide-and-conquer/binary-search-on-the-answer\|Binary search on the answer]] |
| Expected running time | [[review/divide-and-conquer/quicksort-and-its-analysis\|Indicator variables and linearity]] |

## Randomized analysis checklist

- Define one indicator per elementary event, such as "zᵢ and zⱼ are compared".
- Compute each probability from a symmetry argument.
- Sum with linearity of expectation, which needs no independence.
- Say whether the claim is an expectation or a high-probability bound. The [[review/divide-and-conquer/randomized-algorithms-and-probability|concentration inequalities]] differ in what they need.

Use [[exam-prep/practice/algorithms-practice|original practice questions]] 4–6.

Sources: Lectures 4, 5, and 6. See [[setup/source-materials|source materials]].
