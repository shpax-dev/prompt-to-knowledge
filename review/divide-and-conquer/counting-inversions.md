# Counting Inversions

An **inversion** in a list A is a pair of indices i < j with A[i] > A[j]. The number of inversions measures how far A is from sorted: it is 0 for a sorted list and n(n − 1)/2 for a reversed one.

## Three solutions

1. **Check every pair:** for each i, check the n − i later items, for ∑ (n − i) = n(n − 1)/2 = Θ(n²) comparisons.
2. **Insertion sort:** each swap fixes exactly one inversion, so it makes as many swaps as there are inversions and runs in Θ(n + #inversions).
3. **Sort-and-count in O(n log n):** modify [[review/divide-and-conquer/merge-sort-and-the-paradigm|merge sort]].

```text
CountSort(A):  if |A| = 1: return (A, 0)
               (A1, c1) ← CountSort(front half)
               (A2, c2) ← CountSort(back half)
               (A, c)   ← MergeCount(A1, A2)
               return (A, c + c1 + c2)
MergeCount(A, B): pop min{front of A, front of B} into S;
                  if the popped item came from B: c ← c + |A|
```

**Why |A| is right:** `A` here is what is *left* of the front half. When an item from B is placed, it is smaller than every remaining item of A, and each of those earlier-positioned items forms one inversion with it.

The recurrence is the same as merge sort's, so the runtime is O(n log n).

## Derived trace

A = [2, 4, 1, 3, 5]. The halves are [2, 4] and [1, 3, 5], with 0 inversions inside each.

| Step | Pop | Remaining A | Added |
| :--- | :--- | :--- | :--- |
| 1 | 1 (from B) | [2, 4] | +2 |
| 2 | 2 (from A) | [4] | 0 |
| 3 | 3 (from B) | [4] | +1 |
| 4 | 4 (from A) | [] | 0 |
| 5 | 5 (from B) | [] | 0 |

The total is 3, which matches the direct list (2,1), (4,1), (4,3).

**Pitfall:** adding the *original* size of the front half, rather than what remains, overcounts.

Source: `Lecture 4_ Divide and Conquer Strategies Part A.pdf`, PDF pp. 164–177.
