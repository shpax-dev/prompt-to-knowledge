# Primer Preparation

This guide covers Lectures 1–3 and 11. Primer questions test whether you can state a cost model precisely and trace a data structure step by step.

## Work in this order

1. **State the model.** Say what n measures, which operations cost O(1), and whether the bound is worst case, average, or amortized. See [[concepts/primer/problems-and-efficiency|problems and efficiency]].
2. **Compare growth rates.** Reduce each function to a dominant term, then compare with the polynomial, polylog, and exponential rules in [[concepts/primer/asymptotic-notation|asymptotic notation]]. Claim Θ only when you have both an upper and a lower bound.
3. **Amortize sequences.** Choose a charge per operation, say what the stored credit pays for, and give an invariant that keeps the credit non-negative. See [[concepts/primer/amortized-analysis|amortized analysis]].
4. **Trace structures as arrays.**
   - [[concepts/primer/union-find|Union–find]]: write the `parent` array and the root depths after each Union.
   - [[concepts/primer/binary-heaps|Heaps]]: write the 0-based array after each shift.
5. **Graph traversal.** Name the [[concepts/primer/graph-basics-and-traversal|representation, then the traversal]], then the O(m + n) cost. For directed graphs, check whether the question needs [[concepts/primer/strongly-connected-components|SCCs]] or a [[concepts/primer/dags-and-topological-ordering|topological order]].

## Trace table

| Structure | Record at each step |
| :--- | :--- |
| Union–find | Find path, root chosen, new parent, depth change |
| Heap | Array contents, swapped pair, direction (up or down) |
| BFS | Queue contents, current layer, parent pointers |
| DFS / Kosaraju | Stack or recursion order, finish times, SCC label array |

## Common mistakes

- Writing Θ when only O was shown.
- Analyzing a heap Delete only with a shift-down. A non-root deletion may need a shift-up.
- Running Kosaraju's second pass on G instead of G_rev.

Use [[exam-prep/practice/algorithms-practice|original practice questions]] 1–3.

Sources: Lectures 1, 2, 3, and 11. See [[setup/source-materials|source materials]] for filenames and page counts.
