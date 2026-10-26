# Edit Distance and Sequence Alignment

## Edit distance

This is the minimum number of insertions, deletions, and substitutions that change A[1..m] into B[1..n]. For example, TUESDAY → THUESDAY → THURSDAY takes 2 edits. Equivalently, align the strings and count the mismatched columns:

```text
T-UESDAY
THURSDAY
```

**Matrix:** E[i, j] is the edit distance between A[1..i] and B[1..j].

**Bellman equation:** look at the last column of an optimal alignment.

```text
E[i, 0] = i        E[0, j] = j
E[i, j] = min( E[i, j−1] + 1,                    # insert B[j]
               E[i−1, j] + 1,                    # delete A[i]
               E[i−1, j−1] + [A[i] ≠ B[j]] )     # substitute, free if equal
```

**Solution and order:** the answer is E[m, n]. Fill rows 1..m, each across columns 1..n.

**Runtime:** O(mn).

**Space:** each row needs only the previous row, so O(n) space suffices for the value.

## Sequence alignment (Needleman–Wunsch)

This generalizes edit distance with costs:

- An **alignment** M is a set of matched pairs (i, j) with no crossings.
- Each unmatched position, or **gap**, costs δ.
- Matching symbol p with symbol q costs α(p, q).

**Dichotomy lemma:** in any alignment, either (m, n) ∈ M, or position m of X is unmatched, or position n of Y is unmatched. If (m, n) ∉ M and both were matched, they would be matched to earlier positions, and the pairs would cross.

```text
A[i][j] = min( α(xᵢ, yⱼ) + A[i−1][j−1],
               δ + A[i−1][j],
               δ + A[i][j−1] )
```

The runtime is O(mn).

**Graph view:** A[i][j] equals the minimum-cost path from (0, 0) to (i, j) in a grid graph with diagonal edges of cost α and horizontal and vertical edges of cost δ. *Proof:* strong induction on i + j.

## Source example: "mean" versus "name"

The costs are δ = 2 and α = 0 for the same letter, 3 for a vowel paired with a consonant, and 1 otherwise. Rows are prefixes of "mean", written bottom to top; columns are prefixes of "name".

```text
n | 8 6 5 4 6
a | 6 5 3 5 5
e | 4 3 2 4 4
m | 2 1 3 4 6
- | 0 2 4 6 8
    - n a m e
```

The optimal cost is A[4][4] = 6. A derived recomputation reproduces every cell.

**Pitfall:** the edit-distance substitution term is an indicator, 0 when the letters are equal. The alignment version uses α, which need not be 0 or 1.

Source: `Lecture 10_ Dynamic Programming In Depth Examples and Applications.pdf`, PDF pp. 164–221. The alignment table was checked visually on PDF p. 219.
