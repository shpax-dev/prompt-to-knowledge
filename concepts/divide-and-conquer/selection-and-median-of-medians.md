# Selection and Median of Medians

Selection asks for the k-th smallest value in an unsorted array, the element that would be at position k after sorting. Sorting first costs O(n log n); selection can be done in linear time.

## Warm-ups

- **Second largest:** recursively return (max, second max) for each half. The combined second max is the larger of the winning half's second max and the losing half's max. T(n) ≤ 2T(n/2) + Θ(1) = O(n).
- **k largest:** recurse on both halves, keeping k candidates from each, because the top k can be split across the halves. Sort at the size-k leaves. The total is O(n + n log k). A heap-based method is in [[concepts/primer/binary-heaps|binary heaps]].

## Quickselect

```text
QuickSelect(A[1..n], k):
  if n = 1: return A[1]
  choose a pivot; r ← Partition(A, pivot)   # pivot ends at rank r
  if k < r: return QuickSelect(A[1..r−1], k)
  if k > r: return QuickSelect(A[r+1..n], k − r)
  return A[r]
```

Partition costs O(n). Only one side is searched, so:

- **Worst case:** the pivot is always an extreme element, T(n) ≤ T(n − 1) + cn = O(n²).
- **Good case:** if the pivot always lands between ranks 3n/10 and 7n/10, then T(n) ≤ T(7n/10) + cn = O(n).

A constant-fraction split is enough. Splitting at n/10 versus 9n/10, or at n/100 versus 99n/100, is still linear.

## Median of medians (MoM)

The goal is to find a provably good pivot without already knowing the median.

1. Split A into ⌈n/5⌉ groups of 5 and find each group's median by brute force.
2. Recursively select the median `mom` of those medians.
3. Partition around `mom` and recurse into one side, as in quickselect.

**Why the split is balanced:** `mom` is greater than about half the group medians, roughly n/10 of them, and each of those medians is greater than 2 more elements of its group. So `mom` is greater than about 3n/10 elements, and symmetrically smaller than about 3n/10. The larger side has at most about 7n/10 elements.

```text
T(n) ≤ T(n/5) + T(7n/10) + cn = O(n)     (because 1/5 + 7/10 < 1)
```

**Pitfall:** the group size matters. Groups of 3 give T(n/3) + T(2n/3) + cn, where the fractions sum to 1, so the result is Θ(n log n), not linear.

Using MoM as the pivot in [[concepts/divide-and-conquer/quicksort-and-its-analysis|quicksort]] guarantees O(n log n) in the worst case.

Source: `Lecture 6_ Las Vegas Algorithms Randomized Quick Sort and Selection.pdf`, PDF pp. 2–57.
