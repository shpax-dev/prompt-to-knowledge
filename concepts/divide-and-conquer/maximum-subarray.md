## Maximum Subarray

Given an integer array A, find the non-empty contiguous subarray with the largest sum. The course solves it three times, which makes it a good comparison of techniques.

### 1. Brute Force: Θ(n²)

Check every start i and every end j ≥ i. Recomputing each sum from scratch costs O(n³). The trick is to extend a running sum as j increases, so each (i, j) pair costs O(1): ∑ (n − i + 1) = n(n + 1)/2 = Θ(n²).

### 2. Divide and Conquer: O(n Log N)

```text
MaxSubarray(A):  if |A| = 1: return A[1]
                 A1 ← MaxSubarray(front half)
                 A2 ← MaxSubarray(back half)
                 M  ← MidMaxSubarray(A)
                 return the best of A1, A2, M
```

`MidMaxSubarray` finds the best subarray that *crosses* the midpoint m. It extends left from m − 1 for the best suffix, extends right from m for the best prefix, and joins them in O(n).

- *Correctness:* by induction, A1 and A2 are optimal within their halves, and M is the best crossing subarray. Every subarray is in one of those three groups.
- *Runtime:* the merge sort recurrence, O(n log n).

### 3. Dynamic Programming: O(n)

Let s[i] be the best sum of a subarray **ending at** i:

```text
s[i] = max(s[i−1] + A[i], A[i])     answer = maxⱼ s[j]
```

That is O(n) cells, each costing O(1). To recover the subarray, keep `start[i]`: copy start[i−1] when extending, or use i when restarting. Alternatively, trace back from the best j until s[i] = A[i].

**Derived trace:** A = [2, −3, 4, −1, 2, −5, 3] gives s = [2, −1, 4, 3, 5, 0, 3]. The maximum is 5, for the subarray [4, −1, 2].

**Pitfall:** s[i] must end *exactly* at i. Defining it as "the best subarray within A[1..i]" loses the ability to extend.

See [[concepts/dynamic-programming/dynamic-programming-method|the DP method]] for the general template.

Source: `Lecture 5_ Divide and Conquer Strategies Part B.pdf`, PDF pp. 115–125, and `Lecture 10_ Dynamic Programming In Depth Examples and Applications.pdf`, PDF pp. 106–119.
