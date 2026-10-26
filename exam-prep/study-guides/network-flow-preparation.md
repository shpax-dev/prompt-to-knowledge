# Network Flow Preparation

This guide covers Lecture 14. Flow questions come in two kinds: **run the algorithm** and **design a reduction**.

## Running Ford–Fulkerson

For each iteration, record:

| Column | Content |
| :--- | :--- |
| Augmenting path | Its node sequence in G_f, noting any backward edges used |
| Bottleneck | The minimum residual capacity on the path |
| Updated flow | The edges that changed |
| v(f) | The running flow value |

When no augmenting path remains, list the nodes reachable from s in G_f. That set is A* of a minimum cut. Check that c(A*, B*) = v(f), counting only edges from A* to B*. See [[concepts/network-flow/max-flow-and-ford-fulkerson|Ford–Fulkerson]] and [[concepts/network-flow/max-flow-min-cut|max-flow min-cut]].

## Designing a reduction

1. **Nodes:** what does each original object become? Do you need to add s and t?
2. **Edges:** give each edge's direction, capacity, and lower bound if there is one. Capacity 1 usually means "use at most once"; ∞ means "never cut this".
3. **Answer:** is it the flow value, the flow on certain edges, or the minimum cut?
4. **Correctness:** prove both directions. A solution gives a flow of the same value, and an *integral* flow gives a solution.
5. **Runtime:** state the size of the network and the bound C on the flow value.

| Need | Pattern |
| :--- | :--- |
| Pair items, each used at most once | [[concepts/network-flow/bipartite-matching-and-disjoint-paths\|Bipartite matching]] |
| Paths that share no edges | Unit capacities plus path decomposition |
| Two-way partition with penalties | [[concepts/network-flow/min-cut-applications\|Min cut]]: segmentation, project selection |
| "Can team z still win?" | Baseball elimination network |
| Several sources and sinks, or required minimum usage | [[concepts/network-flow/circulations-demands-and-lower-bounds\|Demands and lower bounds]] |

## Common mistakes

- Forgetting backward residual edges, which leaves the algorithm stuck below the maximum.
- Counting B → A edges in a cut's capacity.
- Calling O(mC) polynomial. It is pseudo-polynomial; capacity scaling gives O(m² log C).

Use [[exam-prep/practice/algorithms-practice|original practice questions]] 13–15.

Source: Lecture 14. See [[setup/source-materials|source materials]].
