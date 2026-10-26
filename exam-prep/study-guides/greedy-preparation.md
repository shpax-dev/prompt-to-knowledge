# Greedy Preparation

This guide covers Lectures 7, 8, and 12. Greedy questions reward a short workflow: **propose a rule, try to break it, then prove the survivor.**

## Work in this order

1. **List candidate orders:** earliest, latest, shortest, largest, cheapest, highest ratio.
2. **Hunt for counterexamples** using 2–3 elements. Most bad rules fail on tiny inputs, as in [[concepts/greedy/interval-scheduling|interval scheduling]] and [[concepts/greedy/minimizing-maximum-lateness|lateness]].
3. **Choose a proof technique** from [[concepts/greedy/greedy-proof-techniques|greedy proof techniques]]:
   - **Stays ahead:** works when there is a natural per-step measure, such as finish time or distance.
   - **Exchange:** works when you can swap an adjacent inverted pair, or one element, without making the solution worse.
4. **State what "optimal" means** before proving it. Is it the same size, the same maximum lateness, or the same cost? Several optimal solutions can exist.
5. **Give the implementation cost.** It is usually a sort or a priority queue: O(n log n) or O(m log n).

## Graph greedy

| Question | Tool | Proof idea |
| :--- | :--- | :--- |
| Shortest paths, weights ≥ 0 | [[concepts/greedy/dijkstra-shortest-paths\|Dijkstra]] | Stays ahead; any detour leaves S at a node no closer |
| Minimum spanning tree | [[concepts/greedy/minimum-spanning-trees\|Kruskal / Prim / Reverse-Delete]] | Cut property or cycle property |
| k-clustering | Kruskal stopped at k components | Removes the k − 1 largest MST edges |

## Exchange-proof skeleton

1. Let O be optimal and A be greedy. Suppose O ≠ A.
2. Find the first difference, or an adjacent inversion.
3. Swap, and show the objective does not get worse. Show the arithmetic.
4. Each swap strictly reduces a measure, such as the number of inversions, so finitely many swaps turn O into A.

**Watch for negative weights.** Dijkstra fails on them; switch to [[concepts/dynamic-programming/bellman-ford|Bellman–Ford]].

Use [[exam-prep/practice/algorithms-practice|original practice questions]] 7–9. [[concepts/greedy/huffman-coding|Huffman]] and [[concepts/greedy/offline-caching|caching]] are the harder proofs.

Sources: Lectures 7, 8, and 12. See [[setup/source-materials|source materials]].
