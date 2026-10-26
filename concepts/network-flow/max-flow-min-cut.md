# The Max-Flow Min-Cut Theorem

An **s–t cut** (A, B) partitions V with s ∈ A and t ∈ B. Its **capacity** is the total capacity of edges going from A to B:

```text
c(A, B) = ∑ over edges e leaving A of cₑ
```

Edges from B back to A do not count. Every unit of flow must cross from A to B at some point.

## The chain of results

1. **Flow-value lemma:** for any flow f and any cut (A, B), v(f) = f_out(A) − f_in(A). Proof: sum the conservation equations over the nodes of A. Every node except s contributes 0.
2. **Weak duality:** v(f) ≤ c(A, B), because v(f) = f_out(A) − f_in(A) ≤ f_out(A) ≤ c(A, B).
3. **Theorem:** if G_f has no s–t path, let A* be the set of nodes reachable from s in G_f, and B* = V − A*. Then v(f) = c(A*, B*).
   - Every edge from A* to B* is saturated, f(e) = cₑ; otherwise its head would be reachable.
   - Every edge from B* to A* carries 0 flow; otherwise the backward residual edge would make its tail reachable.
   - So v(f) = ∑ cₑ − 0 = c(A*, B*).
4. **Corollary:** that f is a maximum flow and (A*, B*) is a minimum cut. Anything better would contradict weak duality.
5. **Corollary:** [[concepts/network-flow/max-flow-and-ford-fulkerson|Ford–Fulkerson]] returns a maximum flow, because it stops exactly when G_f has no s–t path.

**Max-flow = min-cut** holds for every flow network.

## Finding a minimum cut

Given a maximum flow, build G_f in O(m). Search from s by BFS or DFS to find A* in O(m + n), and set B* = V − A*. The total is O(m).

## Source example

Network: s→u 20, s→v 10, u→v 30, u→t 10, v→t 20.

- Maximum flow 30: 10 along s→u→t, 10 along s→u→v→t, and 10 along s→v→t.
- Minimum cut ({s}, {u, v, t}), with capacity 20 + 10 = 30.

**Pitfall:** count only edges from A to B in a cut's capacity. Adding the backward edges is the most common arithmetic slip.

The minimum cut, not the flow, is often the real answer to a problem; see [[concepts/network-flow/min-cut-applications|min-cut applications]].

Source: `Lecture 14_ Network Flow and Combinatorial Optimization.pdf`, PDF pp. 10–17 and 66–85.
