# Dynamic Programming Preparation

This guide covers Lectures 9, 10, 12 (Bellman–Ford), and 13. The lecture expects a DP answer to have five parts and **no pseudocode**. See [[review/dynamic-programming/dynamic-programming-method|the DP method]].

## The five-part answer

1. **Definitions:** any preprocessing, such as sorting order or the helper index p(j).
2. **Matrix description:** the dimensions, and what one cell means *in words*.
3. **Bellman equation:** the recurrence from a dichotomy on the last choice, plus base cases.
4. **Solution and fill order:** which cell holds the answer, and why its dependencies are already filled.
5. **Runtime:** preprocessing + cells × work per cell. Also give space, and how to reconstruct the solution.

## Finding the dichotomy

| Problem shape | Dichotomy | Typical table |
| :--- | :--- | :--- |
| Choose a subset of items in order | Item n is in the solution or not | 1-D, or 2-D with a budget ([[review/dynamic-programming/knapsack-and-subset-sum\|knapsack]]) |
| Intervals sorted by finish time | Job j is used, so jump to p(j), or not | 1-D ([[review/dynamic-programming/weighted-interval-scheduling\|weighted intervals]]) |
| Two strings | The last column matches, gaps X, or gaps Y | 2-D prefixes ([[review/dynamic-programming/edit-distance-and-sequence-alignment\|alignment]]) |
| Nested or non-crossing structure | j is unpaired, or paired with t | 2-D intervals ([[review/dynamic-programming/rna-secondary-structure\|RNA]], [[review/dynamic-programming/coins-in-a-line\|coin game]]) |
| Where does the last segment start? | min over the start i | 1-D with an O(n) inner loop ([[review/dynamic-programming/segmented-least-squares\|segmented least squares]]) |
| Paths with negative edges | Use one more edge or not | (edges × nodes) ([[review/dynamic-programming/bellman-ford\|Bellman–Ford]]) |
| All pairs | Vertex k is an intermediate node or not | 3-D, collapsible to 2-D ([[review/dynamic-programming/all-pairs-shortest-paths\|Floyd–Warshall]]) |

## Checks before you submit

- Could greedy work instead? Give a counterexample to justify DP.
- Is the runtime polynomial or **pseudo-polynomial**? Watch for a numeric W or C.
- Fill a small table by hand and confirm the answer cell.
- Trace back at least one step to show reconstruction works.

Use [[exam-prep/practice/algorithms-practice|original practice questions]] 10–12.

Sources: Lectures 9, 10, 12, and 13. See [[setup/source-materials|source materials]].
