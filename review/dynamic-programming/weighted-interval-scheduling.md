# Weighted Interval Scheduling

Each request is (sᵢ, fᵢ, vᵢ): a start, a finish, and a value. The goal is a compatible set of maximum **total value**.

## Greedy fails

The lecture's example has interval 1 with value 1, interval 2 with value 3 overlapping both others, and interval 3 with value 1. [[review/greedy/interval-scheduling|Finish-first]] picks intervals 1 and 3 for value 2, but the optimum is interval 2 alone, with value 3.

## The DP

**Definitions:** sort the requests by finish time. For job j, let p(j) be the largest index i < j with fᵢ ≤ sⱼ, or 0 if there is none.

**Matrix:** a 1-D array M, where M[j] is the best value achievable using only the first j jobs.

**Bellman equation:** the last job is either in the solution or not.

```text
M[0] = 0
M[j] = max( M[j − 1],  M[p(j)] + vⱼ )
```

**Solution and order:** the answer is M[n]. Fill M from left to right.

**Correctness:** strong induction on j. Sorting by finish time guarantees that if job j is used, the remaining jobs come from 1..p(j).

**Runtime:**

- The plain recursion makes O(2ⁿ) calls, because subproblems repeat.
- With memoization: sort in O(n log n), then n cells. Finding p(j) costs O(n) by linear search or O(log n) by binary search, for O(n²) or O(n log n) overall.

**Reconstruction:** job j is in an optimal schedule for 1..j exactly when vⱼ + M[p(j)] ≥ M[j − 1]. If it is, jump to p(j); otherwise move to j − 1.

## Derived trace

Jobs sorted by finish time, as (s, f, v):

| j | Job | p(j) | M[j] |
| :--- | :--- | :--- | :--- |
| 1 | (0, 3, 2) | 0 | max(0, 0 + 2) = 2 |
| 2 | (1, 5, 4) | 0 | max(2, 0 + 4) = 4 |
| 3 | (4, 6, 4) | 1 | max(4, 2 + 4) = 6 |
| 4 | (3, 8, 7) | 1 | max(6, 2 + 7) = 9 |
| 5 | (6, 9, 2) | 3 | max(9, 6 + 2) = 9 |

Trace back from j = 5: since 9 > 8, skip job 5. At j = 4, 9 ≥ 6, so take job 4 and jump to p(4) = 1. Take job 1. The result is {1, 4}, with value 9.

**Pitfall:** p(j) depends on the sorted order. Compute it after sorting, and use ≤, because touching endpoints are compatible.

See [[review/dynamic-programming/dynamic-programming-method|the DP method]] for the answer template.

Source: `Lecture 10_ Dynamic Programming In Depth Examples and Applications.pdf`, PDF pp. 6–50. The greedy counterexample was checked visually on PDF p. 14.
