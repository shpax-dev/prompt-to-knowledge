## Merge Sort and the Divide-and-Conquer Paradigm

### Reductions First

**Reducing** problem X to problem Y means writing an algorithm for X that calls an algorithm for Y as a black box. Long multiplication is a familiar case: it reduces multiplying integers to single-digit products, shifts, and additions. To compute 23 × 19 you need 2·1, 2·9, 3·1, and 3·9. Divide and conquer reduces a problem to *smaller instances of itself*.

### The Paradigm

1. **Divide:** split the problem into smaller, independent subproblems.
2. **Conquer:** solve each subproblem, usually by recursion.
3. **Combine:** build the full solution from the subproblem outputs.

Divide-and-conquer solutions are naturally recursive. Their running times are [[concepts/divide-and-conquer/solving-recurrences|recurrences]]. The technique typically turns an O(n²) brute force into O(n log n).

### Where Sorting Algorithms Sit

| Approach | Examples | Time |
| :--- | :--- | :--- |
| Brute force | Try every ordering | O(n · n!) |
| Simple sorts | Insertion, selection, bubble | O(n²) |
| Divide and conquer | Merge sort, quicksort | Merge sort O(n log n); quicksort O(n²) worst case |
| Non-comparison ("trick") sorts | Radix, counting, with maximum key k | Radix O(n⌈log k⌉); counting O(n + k) |

The lecture asks what key size k makes the non-comparison sorts no better than merge sort. Its printed answer is Ω(n log n), which matches counting sort's O(n + k) bound. For pivot analysis, see [[concepts/divide-and-conquer/quicksort-and-its-analysis|quicksort]].

### Merge Sort

```text
MergeSort(A):  if |A| = 1: return A
               A1 ← MergeSort(front half of A)
               A2 ← MergeSort(back half of A)
               return Merge(A1, A2)
Merge(A, B):   repeatedly pop min{front of A, front of B} into S
```

**Merge correctness:**

- *Soundness:* by induction, after k iterations S holds the k smallest elements in sorted order. Comparing only the two fronts is enough, because each input list is already sorted.
- *Completeness:* the loop runs until both lists are empty, so no element is missed.

**MergeSort correctness:**

- *Soundness:* by strong induction on the list length. A 1-element list is sorted. By the inductive hypothesis A1 and A2 are sorted, and Merge combines them correctly.
- *Completeness:* every call halves the list, so each recursion path reaches the base case.

**Runtime:** Merge is O(n). There are two recursive calls of size n/2, so T(n) ≤ 2T(n/2) + cn = O(n log n).

**Pitfall:** "split in half" hides floors and ceilings. The precise recurrence is T(⌈n/2⌉) + T(⌊n/2⌋) + cn. Assuming n is a power of 2 does not change the asymptotics.

Merge sort's merge step also powers [[concepts/divide-and-conquer/counting-inversions|counting inversions]].

Source: `Lecture 4_ Divide and Conquer Strategies Part A.pdf`, PDF pp. 2–50.
