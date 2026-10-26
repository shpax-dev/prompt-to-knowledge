## Longest Increasing Subsequence

Given an integer array A[1..n], find the longest subsequence whose values strictly increase. A **subsequence** keeps the original order but may skip items; a **substring** must be contiguous. An array has 2ⁿ subsequences, so brute force is exponential.

### Recursive Formulation

```text
LIS(k, A[1..n]) = length of the longest increasing subsequence of A whose values are all > k
  if n = 0: return 0
  if A[1] ≤ k: return LIS(k, A[2..n])                  # A[1] cannot be used
  return max( LIS(k, A[2..n]),                         # skip A[1]
              1 + LIS(A[1], A[2..n]) )                 # take A[1]
```

Call it as LIS(−∞, A[1..n]). It runs in O(2ⁿ), but there are only O(n²) *distinct* calls: k is always −∞ or some A[i], and the suffix is always some A[j..n].

### The DP

**Matrix:** a 2-D array L, where L[i, j] is the length of the longest increasing subsequence of A[j..n] using only values greater than A[i], for i < j. Add a sentinel A[0] = −∞.

**Bellman equation:**

```text
L[i, j] = 0                                     if j > n
L[i, j] = L[i, j+1]                             if A[i] ≥ A[j]
L[i, j] = max( L[i, j+1], L[j, j+1] + 1 )       otherwise
```

**Solution and order:** the answer is L[0, 1]. Fill j from n down to 1, and for each j fill i from 0 to j − 1, in either direction.

**Runtime:** O(n²) cells at O(1) each, so O(n²).

*Derived alternative:* let E[j] be the length of the longest increasing subsequence that *ends* at j. Then E[j] = 1 + max over i < j with A[i] < A[j] of E[i]. This is also O(n²), with a 1-D table.

**Derived check:** A = [3, 1, 4, 1, 5, 9, 2, 6] has LIS length 4, for example 3, 4, 5, 9 or 1, 4, 5, 6.

**Pitfall:** "increasing" here means strictly increasing (A[iₖ] < A[iₖ₊₁]). Allowing ties changes the comparison in the recurrence.

Source: `Lecture 10_ Dynamic Programming In Depth Examples and Applications.pdf`, PDF pp. 56–76.
