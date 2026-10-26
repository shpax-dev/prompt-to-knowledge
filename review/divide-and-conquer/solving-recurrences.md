# Solving Recurrences

A divide-and-conquer running time is a recurrence: a recursive cost plus the work to divide and combine. Merge sort's is T(n) ≤ 2T(n/2) + cn with T(1) ≤ c. Floors and ceilings can usually be ignored asymptotically, which amounts to assuming n is a power of 2.

## Method 1: unroll

```text
T(n) ≤ 2T(n/2) + cn
     ≤ 4T(n/4) + 2cn
     ≤ 2^k T(n/2^k) + k·cn
```

Stop when n/2^k = 1, that is when k = log₂ n: T(n) ≤ n·T(1) + cn log n = O(n log n).

## Method 2: recursion tree

Level i has 2^i nodes, each costing cn/2^i, so every level costs cn. There are log₂ n + 1 levels, so the total is cn lg n + cn = O(n log n).

## Method 3: guess and prove by induction

Guess T(n) ≤ cn lg n + cn, then prove it.

- *Base case n = 2:* T(2) = 2T(1) + 2c ≤ 4c = c·2·lg 2 + 2c.
- *Inductive step:* T(k) ≤ 2(c(k/2) lg(k/2) + c(k/2)) + ck = ck lg k − ck + 2ck = ck lg k + ck.

**Pitfall:** a guess is not an answer until the induction goes through with the *same* constant.

## Method 4: the q-subproblem pattern

For T(n) ≤ q·T(n/2) + cn:

| q | T(n) |
| :--- | :--- |
| 1 | O(n) |
| 2 | O(n log n) |
| q > 2 | O(n^(lg q)) |

## Method 5: Master theorem

For T(n) = a·T(n/b) + f(n) with a ≥ 1 and b > 1, compare f(n) with n^(log_b a):

1. If f(n) = O(n^(log_b a − ε)) for some ε > 0, then T(n) = Θ(n^(log_b a)). The leaves dominate.
2. If f(n) = Θ(n^(log_b a)), then T(n) = Θ(n^(log_b a) log n). Every level costs about the same.
3. If f(n) = Ω(n^(log_b a + ε)) and a·f(n/b) ≤ c·f(n) for some c < 1, then T(n) = Θ(f(n)). The root dominates.

## Method 6: Akra–Bazzi

This handles unequal splits: T(n) = ∑ aᵢT(n/bᵢ) + f(n). Find p with ∑ aᵢ/bᵢ^p = 1. Then

```text
T(n) = Θ( n^p · (1 + ∫₁ⁿ f(u)/u^(p+1) du) )
```

It applies to [[review/divide-and-conquer/selection-and-median-of-medians|median of medians]]: T(n/5) + T(7n/10) + n. There p < 1 because 1/5 + 7/10 < 1, which gives Θ(n).

## Quick reference

| Recurrence | Result | Example |
| :--- | :--- | :--- |
| T(n−1) + Θ(1) | Θ(n) | Linear search |
| T(n/2) + Θ(1) | Θ(log n) | Binary search, fast exponentiation |
| 2T(n/2) + Θ(1) | Θ(n) | Second-largest element |
| 2T(n/2) + Θ(n) | Θ(n log n) | Merge sort, closest pair |
| 3T(n/2) + Θ(n) | Θ(n^(lg 3)) | Karatsuba |
| 7T(n/2) + Θ(n²) | Θ(n^(lg 7)) | Strassen |

See [[review/divide-and-conquer/binary-search-on-the-answer|binary search]] and [[review/divide-and-conquer/fast-multiplication|fast multiplication]] for the logarithmic and sub-quadratic rows.

Source: `Lecture 4_ Divide and Conquer Strategies Part A.pdf`, PDF pp. 51–97. Examples from `Lecture 5_ Divide and Conquer Strategies Part B.pdf` and `Lecture 6_ Las Vegas Algorithms Randomized Quick Sort and Selection.pdf`.
