# The Dynamic Programming Method

Dynamic programming (DP) is "extreme divide and conquer". It uses many overlapping subproblems, but far fewer than brute force, and it solves them from smallest to largest while storing each answer. Richard Bellman chose the name in the 1950s, when "programming" meant planning, and "dynamic" sounded exciting to his Air Force sponsors.

## Why remembering matters

- **Factorial:** n! = (n − 1)!·n needs M(n) = M(n − 1) + 1 = n multiplications. Each smaller value is needed exactly once.
- **Naive Fibonacci:** F(n) = F(n − 1) + F(n − 2) recomputes the same values exponentially often. The number of additions satisfies A(n) = A(n − 1) + A(n − 2) + 1, with A(0) = A(1) = 0.
  - **Bounds:** 2^(n/2) ≲ A(n) ≲ 2ⁿ.
  - **Exact count by homogenization:** B(n) = A(n) + 1 satisfies the Fibonacci recurrence itself, and B(n) is the size of the recursion tree. *Derived closed form:* A(n) = F(n + 1) − 1, checked for n ≤ 7.

n! is a much bigger number than F(n), yet it is far cheaper to compute. The difference is not the recurrence's form. It is whether subproblems repeat. Storing them is **memoization**, a term Donald Michie coined in 1989 from the Latin *memorandum*: compute once, store, and look up. It can be written recursively, but it is usually more natural as a loop.

*Derived aside:* the lecture asks how to compute F(n) in O(log n). One way is to raise the 2 × 2 matrix with rows (1, 1) and (1, 0) to the n-th power using [[concepts/divide-and-conquer/fast-multiplication|fast exponentiation]].

## Shortest paths in a DAG: the template

Process the nodes in [[concepts/primer/dags-and-topological-ordering|topological order]]:

```text
d(x) = min over edges (u, x) of d(u) + w(u, x)
```

Every predecessor of x is finished before x, so each d(x) is computed exactly once. Two lessons carry over to all DP:

1. You need an **order** in which smaller subproblems come first, which the DAG provides.
2. You need a **relation** for each subproblem in terms of earlier ones. Here it is min-plus, but max-plus (longest path) or min-product work the same way.

DP is shortest or longest path computation on an implicit DAG of subproblems: an edge A → B means B needs A's answer.

## Comparing the three paradigms

| Paradigm | Subproblems | Recursion tree, if unmemoized |
| :--- | :--- | :--- |
| Divide and conquer | Much smaller (e.g. n/2), non-overlapping | Depth O(log n), O(n) nodes |
| Dynamic programming | Slightly smaller (e.g. n − 1), overlapping | Depth O(n), exponentially many nodes |
| Greedy | Slightly smaller, but each optimum depends only on the previous one plus a local choice | A single chain, O(n) nodes |

Greedy is like factorial: once the data is sorted by the right criterion, Opt(n) = Opt(n − 1) ⊕ local choice.

## How to write a DP solution

The lecture's rule is "no pseudocode". A DP answer gives:

1. **Definitions** the algorithm needs, such as sorting order or helper indices.
2. **Matrix description:** the dimensions, and what cell [i, j] *means* in words.
3. **Bellman equation:** the recurrence, plus base cases.
4. **Solution location and fill order:** which cell holds the answer, and an order in which every dependency is filled first.
5. **Runtime:** preprocessing + (number of cells × work per cell).
6. **Reconstruction:** how to trace back the actual solution, not just its value.

Good DPs have a polynomial number of subproblems, an efficient way to combine them, and a natural small-to-large order.

Worked DPs: [[concepts/divide-and-conquer/maximum-subarray|maximum subarray]] (the same problem solved three ways), [[concepts/dynamic-programming/weighted-interval-scheduling|weighted intervals]], [[concepts/dynamic-programming/longest-increasing-subsequence|LIS]], [[concepts/dynamic-programming/knapsack-and-subset-sum|knapsack]], [[concepts/dynamic-programming/edit-distance-and-sequence-alignment|edit distance]], [[concepts/dynamic-programming/coins-in-a-line|coins in a line]], [[concepts/dynamic-programming/segmented-least-squares|segmented least squares]], [[concepts/dynamic-programming/rna-secondary-structure|RNA structure]], [[concepts/dynamic-programming/bellman-ford|Bellman–Ford]], and [[concepts/dynamic-programming/all-pairs-shortest-paths|all-pairs shortest paths]].

Source: `Lecture 9_ Dynamic Programming A Primer.pdf`, PDF pp. 2–92. The answer-writing template is from `Lecture 10_ Dynamic Programming In Depth Examples and Applications.pdf`, PDF pp. 25–37 and 51–55.
