# Quicksort and Its Analysis

```text
QuickSort(A[1..n]):
  choose a pivot; r ← Partition(A, pivot)     # O(n)
  QuickSort(A[1..r−1]); QuickSort(A[r+1..n])
```

There is no combine step, because quicksort sorts in place.

## Deterministic cases

| Case | Recurrence | Result |
| :--- | :--- | :--- |
| Worst (pivot is always an extreme) | T(n) ≤ T(n−1) + T(0) + O(n) | O(n²) |
| Best (pivot is always the median) | T(n) ≤ 2T(n/2) + O(n) | O(n log n) |
| Any constant split α : (1 − α) | T(αn) + T((1−α)n) + Θ(n) | Θ(n log n) |
| Median-of-medians pivot | T(7n/10) + T(3n/10) + O(n) | O(n log n) worst case |

The median-of-medians pivot is explained in [[concepts/divide-and-conquer/selection-and-median-of-medians|selection]].

**Average case:** if every input ordering is equally likely, T(n) ≤ (2/n)·∑ T(i − 1) + O(n), which gives O(n log n). The weak point is the uniform-input assumption. The easy fix is to **choose the pivot uniformly at random**, which turns quicksort into a Las Vegas algorithm with that expected bound on every input.

## Expected comparisons with a random pivot

Let z₁ < z₂ < … < zₙ be the elements in sorted order. The total running time is O(n + X), where X is the number of comparisons.

1. **Indicators:** Xᵢⱼ = 1 if zᵢ and zⱼ are ever compared. Then X = ∑ over i < j of Xᵢⱼ.
2. **Key observation:** zᵢ and zⱼ are compared exactly when one of them is the *first* pivot chosen among zᵢ, zᵢ₊₁, …, zⱼ. Until then they share a subarray. If some zₖ strictly between them is picked first, they are separated forever.
3. **Probability:** that range has j − i + 1 elements, each equally likely to be chosen first, so Pr[Xᵢⱼ = 1] = 2/(j − i + 1).
4. **Linearity of expectation:**

```text
E[X] = ∑ over i<j of 2/(j − i + 1) = 2(n + 1)Hₙ − 4n
```

The closed form is a derived simplification, checked exactly for n = 2, 3, and 10. Here Hₙ = 1 + 1/2 + … + 1/n is the harmonic number, and ln(n + 1) < Hₙ < 1 + ln n, which you can see by comparing the sum with the integral of 1/x. So E[X] = Θ(n log n).

## Concentration

An expected bound does not say how often the algorithm is slow. The lecture applies the [[concepts/divide-and-conquer/randomized-algorithms-and-probability|concentration inequalities]]:

- **Markov:** Pr[T ≥ k·E[T]] ≤ 1/k. With k = 10, the run stays within 10 times its expected cost at least 90% of the time.
- **Chebyshev:** using an estimate Var[T] = O(n²), large deviations become polynomially unlikely.
- **High probability (homework):** Pr[T ≥ (1 + δ)E[T]] ≤ 1/n^c, after working around the dependence between the Xᵢⱼ.

**Pitfall:** "expected O(n log n)" is about the algorithm's coins, not the input. A malicious input cannot force the worst case, but an unlucky sequence of coin flips still can, with small probability.

Source: `Lecture 6_ Las Vegas Algorithms Randomized Quick Sort and Selection.pdf`, PDF pp. 58–82 and 118–154. The lecture's own closed-form line on PDF p. 128 does not extract cleanly. The Θ(n log n) conclusion is the source's; the exact sum above is a derived check.
