## Dijkstra's Shortest Paths

Input: a directed graph G = (V, E) with n nodes and m edges, a source s that reaches every node, and edge lengths ℓₑ ≥ 0. Goal: a shortest path from s to every node.

### Algorithm (Dijkstra, 1956)

```text
S ← {s}; d(s) ← 0
while S ≠ V:
  choose v ∉ S minimizing d′(v) = min over edges (u, v) with u ∈ S of d(u) + ℓ(u, v)
  add v to S; d(v) ← d′(v)
```

It is greedy because it always finalizes the closest unexplored node. You can think of it as BFS with continuous, weighted distances.

### Correctness (Stays Ahead)

**Theorem:** at every point, for each u ∈ S, the path Pᵤ the algorithm found is a shortest s–u path.

*Induction on |S|:*

- The base case is S = {s}.
- When v is added, Pᵥ is the cheapest path that leaves S by a single edge.
- Any other s–v path must leave S somewhere, say at a node y ∉ S. Because lengths are non-negative, that path already costs at least d′(y) ≥ d′(v) when it reaches y. If it were shorter, y would have been chosen instead of v.

### Negative Edges Break it

The guarantee that the cheapest edge out of S is part of a shortest path fails.

**Derived example:** A → B costs 2, A → C costs 3, C → B costs −2. Dijkstra finalizes d(B) = 2, but A → C → B costs 1.

Adding a constant to every edge does not fix this. A path with x edges gains x times the constant, so paths with different numbers of edges are penalized differently. Use [[concepts/dynamic-programming/bellman-ford|Bellman–Ford]] instead.

### Runtime

There are n − 1 iterations.

| Implementation | Time |
| :--- | :--- |
| Scan every edge for the minimum each round | O(mn) |
| Binary-heap priority queue with decrease-key | O(m log n) |
| Fibonacci heap | O(m + n log n) |

The binary-heap version is the same priority-queue pattern as [[concepts/greedy/minimum-spanning-trees|Prim's algorithm]], with a different key.

**Related:** [[concepts/primer/graph-basics-and-traversal|BFS]] computes shortest paths when every edge has length 1.

### What Each Exploration Builds

- **BFS/DFS:** a tree, with no distance guarantee.
- **Dijkstra:** a tree whose s–u paths are all shortest paths.
- **MST algorithms:** a tree of minimum total cost connecting every node.

Source: `Lecture 12_ Shortest Paths and Minimum Spanning Trees.pdf`, PDF pp. 2–35 and 125–127. The Fibonacci-heap bound is from `Lecture 13_ All Pair Shortest Paths.pdf`, PDF p. 41.
