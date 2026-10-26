# Bipartite Matching and Edge-Disjoint Paths

These are the first examples of **reducing a problem to max flow**. A good reduction answers three questions:

1. **Encoding:** what are the nodes and edges? Are s and t already present, or do you add them?
2. **Edges:** what is each edge's direction, and what is its capacity?
3. **Correspondence:** does every flow give a valid solution, and every solution give a flow of equal value? Is the construction efficient?

## Maximum bipartite matching

Input: a bipartite graph G = (X ∪ Y, E). A **matching** is a set of edges in which no node appears twice. The goal is a largest matching.

**Construction G′:**

- Direct every original edge from X to Y.
- Add s with an edge to every node of X, and t with an edge from every node of Y.
- Give every edge capacity 1.

**Theorem:** the size of a maximum matching in G equals the max-flow value in G′. The original edges that carry flow form a maximum matching.

- s sends at most 1 unit into each x, so x uses at most one outgoing edge.
- Conservation means each y receives at most 1 unit, from a single x.
- Integrality makes every edge's flow either 0 or 1.

**Runtime:** with n = |X| = |Y| and m = |E|, the flow value is at most n, so plain Ford–Fulkerson runs in O(mC) = O(mn).

## Edge-disjoint paths

Find the largest number of s–t paths that share no edge.

- **Directed graph:** give every edge capacity 1. If there are k edge-disjoint paths, the max flow is k, and the converse also holds.
- **Undirected graph:** replace each edge {u, v} with two directed edges (u, v) and (v, u), then proceed as above.
- **Runtime:** O(mn).

**Recovering the paths (path decomposition):** search from s along edges carrying flow 1.

- If you reach t on a simple path P, record P, zero the flow on P, and search again from s.
- If you close a cycle C before reaching t, zero the flow on C, which removes nothing useful, and continue from the start of the cycle.

**Pitfall:** prove *both* directions of the correspondence. Showing that a matching gives a flow is not enough; you also need that an integral flow gives a matching of the same size.

Further reductions with demands and lower bounds are in [[review/network-flow/circulations-demands-and-lower-bounds|circulations]].

Source: `Lecture 14_ Network Flow and Combinatorial Optimization.pdf`, PDF pp. 86–107.
