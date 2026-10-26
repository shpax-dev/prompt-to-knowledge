# Coins in a Line (DP for Games)

An even number n of coins with values sit in a line. Alice and Bob alternate turns, starting with Alice. On each turn a player takes the coin at either the head or the tail. Whoever collects more total value wins. DP is a natural way to find optimal strategies in games like this.

## Greedy strategies

- **Take the larger end coin:** this can lose. For [1, 3, 6, 3], Alice takes 3, Bob takes 6, Alice takes 3, and Bob takes 1, so Alice has 6 and Bob has 7.
- **Even/odd strategy:** Alice can always take all the even-position coins, or all the odd-position ones, whichever set is worth more. So she never loses. For [1, 3, 6, 3, 1, 3], the even positions sum to 9 and the odd positions to 8. But that is not optimal: a better line of play gives Alice 10.

## The DP

**Dichotomy:** Alice takes the head or the tail, and then Bob plays optimally, leaving her the *worse* of his two choices.

**Matrix:** M[i, j] is the most Alice can collect from coins c[i..j] when it is her turn and Bob plays optimally.

**Bellman equation:**

```text
M[i, i] = c[i]
M[i, i+1] = max(c[i], c[i+1])
M[i, j] = max( c[i] + min(M[i+2, j],   M[i+1, j−1]),
               c[j] + min(M[i+1, j−1], M[i, j−2]) )
```

**Solution and order:** the answer is M[1, n]. Fill shorter intervals first: i from n − 2 down to 1, and j from i + 2 up to n.

**Runtime:** O(n²) cells at O(1) each, so O(n²).

**Correctness:** strong induction in the order the cells are filled.

**Derived check:** for [1, 3, 6, 3, 1, 3], M[1, 6] = 10 out of 17. For [1, 3, 6, 3], M[1, 4] = 7 out of 13, so the larger-end greedy above was not optimal for Alice.

**Pitfall:** the inner `min` models an optimal opponent. Replacing it with `max` silently assumes Bob helps Alice.

See [[review/dynamic-programming/dynamic-programming-method|the DP method]] for the general template.

Source: `Lecture 10_ Dynamic Programming In Depth Examples and Applications.pdf`, PDF pp. 77–105.
