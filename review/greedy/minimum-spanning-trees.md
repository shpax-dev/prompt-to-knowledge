# Minimum Spanning Trees

Input: a connected undirected graph G = (V, E) with n nodes, m edges, and edge costs cₑ > 0. Goal: an edge set F of minimum total cost that keeps the graph connected.

**Observation:** an optimal F forms a tree. If it contained a cycle, removing any edge of that cycle would keep the graph connected and lower the cost.

**Assume distinct costs.** Then the MST is unique (a homework exercise). With ties, use a consistent tie-breaker, such as ordering by the vertex labels.

## Two properties

- **Cut property:** let S be a non-empty proper subset of V, and let e be the cheapest edge with exactly one endpoint in S. Then *every* MST contains e.
  - *Exchange:* take a spanning tree T without e = (v, w). The path from v to w in T crosses the cut at some edge e′. Then T − e′ + e is still connected and cheaper.
- **Cycle property:** the most expensive edge on any cycle C is in *no* MST.
  - *Exchange:* if T contains that edge e, removing it splits T into two parts. Some other edge e′ of C reconnects them, and T − e + e′ is cheaper.

## Three greedy algorithms

| Algorithm | Rule | Proof |
| :--- | :--- | :--- |
| Kruskal (1956) | Add edges from cheapest to most expensive, skipping any that would create a cycle | Each added edge is the cheapest across the cut around one of its endpoint's components (cut property) |
| Prim (1957; also Jarník 1929) | Grow one tree from s, always adding the cheapest edge leaving it | Exactly the cut property |
| Reverse-Delete | Remove edges from most expensive to cheapest, unless removal disconnects the graph | Each removed edge is the most expensive on some cycle (cycle property) |

## Implementations

- **Prim:** keep a [[review/primer/binary-heaps|priority queue]] of the cheapest connection to each node outside the tree. That is n − 1 ExtractMins and up to m key changes, each O(log n), for O(m log n). It is almost identical to [[review/greedy/dijkstra-shortest-paths|Dijkstra]]; only the quantity being minimized differs.
- **Kruskal:** sort the edges in O(m log m) = O(m log n), since m ≤ n². Then use [[review/primer/union-find|union–find]] to test whether an edge's endpoints are already connected: 2m Finds at O(log n) each, or nearly O(1) with path compression, and n − 1 Unions at O(1) each.

## Application: k-clustering with maximum spacing

Partition n objects into k non-empty groups so that the smallest distance between two different groups is as large as possible. Run Kruskal and stop once k components remain. That is the same as removing the k − 1 most expensive MST edges. The runtime is O(n² log n) on the complete distance graph.

**Derived example:** undirected edges a–c 1, b–c 2, d–e 2, e–f 3, a–b 4, b–d 5, d–f 6, c–d 8, c–e 10. Kruskal keeps a–c, b–c, d–e, e–f, and b–d, for total cost 13. It skips a–b (cycle a–b–c) and d–f (cycle d–e–f).

Source: `Lecture 12_ Shortest Paths and Minimum Spanning Trees.pdf`, PDF pp. 58–134.
