# RNA Secondary Structure

An RNA strand B = b₁b₂…bₙ over the alphabet {A, C, G, U} folds back on itself and forms base pairs. A **secondary structure** is a set S of pairs (i, j) with i < j such that:

1. **No sharp turns:** i < j − d, for a fixed constant d.
2. **Valid pairs only:** each pair is (A, U), (U, A), (C, G), or (G, C).
3. **Matching:** no base appears in more than one pair.
4. **Non-crossing:** no two pairs (i, j) and (i′, j′) have i < i′ < j < j′.

The goal is a secondary structure with the maximum number of pairs.

## Why 1-D fails

Try m[j], the best for b₁..bⱼ. If j pairs with some t, non-crossing splits the problem into b₁..bₜ₋₁ and bₜ₊₁..bⱼ₋₁. The second piece does not start at 1, so a prefix table cannot describe it.

## The DP (intervals)

**Matrix:** m[i][j] is the maximum number of pairs within bᵢ..bⱼ. It is 0 whenever i ≥ j − d.

**Bellman equation:** either j is unpaired, or j pairs with some t, where i ≤ t < j − d and v(t, j) = 1 if the pair is valid.

```text
m[i][j] = max( m[i][j−1],
               max over i ≤ t < j−d of v(t, j) · (1 + m[i][t−1] + m[t+1][j−1]) )
```

**Solution and order:** the answer is m[1][n]. Fill by increasing interval length.

**Runtime:** O(n²) cells at O(n) each, so O(n³).

## Source example

B = ACCGGUAGU with d = 4. The final table gives m[1][9] = 2. *Derived witness:* the nested pairs (1, 9) A–U and (2, 8) C–G. Both satisfy i < j − 4, and they do not cross.

**Pitfall:** the sharp-turn rule makes short intervals zero. Forgetting it lets the DP pair neighbors that cannot physically fold.

Interval DPs like this one are also used for [[concepts/dynamic-programming/coins-in-a-line|coins in a line]].

Source: `Lecture 10_ Dynamic Programming In Depth Examples and Applications.pdf`, PDF pp. 238–273. The lecture marks this topic with an asterisk in its navigation bar.
