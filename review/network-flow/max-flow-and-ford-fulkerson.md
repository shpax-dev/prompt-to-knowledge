# Max Flow and Ford–Fulkerson

A **flow network** is a directed graph G = (V, E) with a source s, a sink t, and a capacity cₑ ≥ 0 on each edge. Think of water or traffic moving from s to t. A very wide range of problems can be cast as flow problems; see [[review/network-flow/bipartite-matching-and-disjoint-paths|matching]] and [[review/network-flow/min-cut-applications|min-cut applications]].

A **flow** f: E → ℝ≥0 must satisfy:

1. **Capacity:** 0 ≤ f(e) ≤ cₑ for every edge.
2. **Conservation:** f_in(v) = f_out(v) for every v other than s and t.

The **value** of the flow is v(f) = f_out(s) = f_in(t). The **max-flow problem** asks for a flow of maximum value.

## Why naive greedy fails

Repeatedly pushing flow along any s–t path with spare capacity can get stuck. In the lecture's network (s→u 20, s→v 10, u→v 30, u→t 10, v→t 20), pushing 20 along s→u→v→t leaves no path with spare capacity at value 20. The maximum is 30. Greedy needs a way to *undo* flow.

## Residual graph

For a flow f, the residual graph G_f has the same nodes. For each edge (u, v) of G:

- a **forward** edge (u, v) with capacity cₑ − f(e), the room left to push more;
- a **backward** edge (v, u) with capacity f(e), the amount that can be undone.

An **augmenting path** is a simple s–t path in G_f. Its **bottleneck** is the smallest residual capacity along it. Pushing q = bottleneck along the path raises the flow by q on forward edges and lowers it by q on backward edges.

## Ford–Fulkerson (1956)

```text
f(e) ← 0 for every edge
while G_f has an augmenting path P:
  push bottleneck(P, G_f) along P
```

- **Integrality:** with integer capacities, every flow value and residual capacity stays an integer.
- **Progress:** each augmentation raises v(f) by the bottleneck, which is at least 1.
- **Termination:** with C = ∑ of capacities leaving s, there are at most C iterations.
- **Runtime:** each iteration finds a path by BFS or DFS in O(m), since m ≥ n − 1 in a connected graph. It updates the path in O(n) and rebuilds G_f in O(m). The total is **O(mC)**, which is *pseudo-polynomial*.
- **Optimality:** the loop stops exactly when there is no augmenting path, and [[review/network-flow/max-flow-min-cut|max-flow min-cut]] shows that the flow is then maximum.

## Capacity scaling

Choosing paths with large bottlenecks helps. In the network s→u 100, s→v 100, u→v 1, u→t 100, v→t 100, an unlucky choice keeps routing 1 unit through the middle edge.

```text
Δ ← largest power of 2 ≤ max capacity out of s
while Δ ≥ 1:
  while G_f(Δ), which keeps only residual edges ≥ Δ, has an augmenting path: augment
  Δ ← Δ/2
```

There are 1 + ⌈lg C⌉ phases, each with O(m) augmentations costing O(m) apiece, for **O(m² log C)**. That *is* polynomial, because log C is the number of bits needed to write C. The last phase has Δ = 1, so the result is a maximum flow.

## Strongly polynomial algorithms

A strongly polynomial bound depends only on n and m, not on the capacities. The lecture lists:

- Edmonds–Karp (BFS, fewest-edges augmenting paths, 1970): O(m²n)
- Dinitz (1970)
- Preflow-push (1974/1986): O(n³)
- Orlin (2013): O(mn)

## Min-cost max-flow

Give each edge a cost per unit, $ₑ ≥ 0. Among all maximum flows, find the cheapest. Augment along the *cheapest* augmenting path, where a backward residual edge costs −$ₑ. With negative edge costs, find that path with [[review/dynamic-programming/bellman-ford|Bellman–Ford]]. One can show no negative cycles arise, and costs can be reweighted so that Dijkstra works.

Source: `Lecture 14_ Network Flow and Combinatorial Optimization.pdf`, PDF pp. 2–65 and 195–204.
