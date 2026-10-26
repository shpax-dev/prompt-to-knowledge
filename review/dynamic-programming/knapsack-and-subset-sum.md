# Knapsack and Subset Sum

## Subset sum

A machine is available for time W, and jobs have run times w₁, …, wₙ. Choose a subset S that maximizes ∑ over i ∈ S of wᵢ, subject to that sum being ≤ W.

**Greedy fails:**

- *Decreasing weights:* for {W/2 + 1, W/2, W/2}, greedy takes W/2 + 1 and then nothing else fits. The optimum is W/2 + W/2 = W.
- *Increasing weights:* for {1, W/2, W/2}, greedy takes 1 and W/2, getting W/2 + 1 instead of W.

**A 1-D table is not enough.** If item n is used, the remaining items face a *smaller* budget W − wₙ. So the subproblem must track the remaining capacity as well as the item prefix.

## The DP (two dimensions)

**Matrix:** v[i, w] for items 0..n and capacities 0..W. It is the best achievable sum using only the first i items with capacity w.

**Bellman equation:** let xᵢ,w = 1 if wᵢ ≤ w, and 0 otherwise.

```text
v[0, w] = 0,  v[i, 0] = 0
v[i, w] = max( v[i−1, w],  xᵢ,w · (v[i−1, w − wᵢ] + wᵢ) )
```

**Solution and order:** the answer is v[n, W]. Fill row by row.

**Runtime:** O(nW).

**Reconstruction:** walk back from (n, W). If v[i, w] ≠ v[i − 1, w], item i was used, so move to (i − 1, w − wᵢ). This takes O(n).

## Knapsack

Each item now has a weight wᵢ and a separate value vᵢ. Maximize total value subject to total weight ≤ W. The only change is in the "take" branch:

```text
v[i, w] = max( v[i−1, w],  xᵢ,w · (v[i−1, w − wᵢ] + vᵢ) )
```

## Pseudo-polynomial, not polynomial

O(nW) is polynomial in the *numeric value* W, but W takes only about log W bits to write down. If W is around 2ⁿ, the table is exponential in the input size. Subset sum is in fact [[review/np-completeness/classic-np-complete-reductions|NP-complete]], and that does not conflict with this DP.

## Derived trace

W = 5, with items (weight, value) = (2, 3), (3, 4), (4, 5), (5, 6).

| Row i (item) / capacity w | 0 | 1 | 2 | 3 | 4 | 5 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 1 (2, 3) | 0 | 0 | 3 | 3 | 3 | 3 |
| 2 (3, 4) | 0 | 0 | 3 | 4 | 4 | 7 |
| 3 (4, 5) | 0 | 0 | 3 | 4 | 5 | 7 |
| 4 (5, 6) | 0 | 0 | 3 | 4 | 5 | 7 |

The optimum is 7, using items 1 and 2. Tracing back: v[4,5] = v[3,5] = v[2,5] = 7 ≠ v[1,5] = 3, so item 2 is used. Move to (1, 2), where v[1,2] = 3 ≠ v[0,2], so item 1 is used.

Source: `Lecture 10_ Dynamic Programming In Depth Examples and Applications.pdf`, PDF pp. 120–163. Pseudo-polynomial framing: `Lecture 15_ NP Reductions and Intractability.pdf`, PDF pp. 168–169.
