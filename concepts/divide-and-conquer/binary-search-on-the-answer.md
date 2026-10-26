# Binary Search and Searching the Answer Space

## Binary search on a sorted array

Compare the target with the middle element and recurse into the half that can still contain it.

```text
T(n) = T(n/2) + Θ(1) = Θ(log n)
```

Linear search on an unsorted array is T(n) = T(n − 1) + Θ(1) = Θ(n). Both bounds are optimal for their settings; see [[concepts/divide-and-conquer/lower-bounds|lower bounds]].

## Binary search on the answer

Binary search also works on a **solution space**, as long as feasibility is monotone. Order the candidate costs 1, 2, …, k. If cost k* is achievable, every larger cost is achievable too:

```text
cost:      1   2   3   …   k*−1  |  k*   k*+1  …
feasible:  ✗   ✗   ✗   …    ✗    |  ✓     ✓    …
```

Instead of asking "what is the optimum?", ask the decision question "is there a solution with cost at most k?" Binary-search for the boundary instead of stepping k down one value at a time.

## Example: splitting a trip

You have k days and n stops in order, with leg distances d₁, …, dₙ. You may rest only at stops. Minimize the largest single-day distance.

- **Search range:** from max dᵢ (no day can be shorter than its longest leg) up to ∑ dᵢ (everything in one day).
- **Feasibility test for a cap D:** scan the legs in order, starting a new day whenever the next leg would exceed D. The cap is feasible if this uses at most k days. This is a linear scan.
- **Runtime:** O(n log D), where D is the size of the range of candidate answers.

**Derived example:** legs [4, 2, 7, 3, 5], k = 3.

| Cap D | Greedy days | Feasible? |
| :--- | :--- | :--- |
| 7 | [4,2] [7] [3] [5] → 4 | no |
| 8 | [4,2] [7] [3,5] → 3 | yes |
| 9 | [4,2] [7] [3,5] → 3 | yes |

The optimum is D = 8.

**Pitfall:** the method needs monotone feasibility and a feasibility test that is correct for every cap. Verify both before you binary-search.

The same decision/optimization link reappears in [[concepts/np-completeness/polynomial-time-reductions|polynomial-time reductions]].

Source: `Lecture 4_ Divide and Conquer Strategies Part A.pdf`, PDF pp. 85–97 and 154–163.
