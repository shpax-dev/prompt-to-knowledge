# Graph Basics and Traversal

A graph G = (V, E) is a set of vertices and a set of edges joining pairs of vertices. Write n = |V| and m = |E|. Graphs model transportation, communication, information, social, and dependency networks.

## Vocabulary

- **Special graphs:** complete graphs, cycles, paths, trees and forests, bipartite graphs, digraphs, and DAGs.
- **Tree:** a connected graph with no cycles. It has exactly one path between any two nodes and |V| = |E| + 1. An unrooted tree with at least 2 nodes has at least 2 leaves.
- **s–t connectivity:** is there a path from s to t?
- **Connected component:** a connected subgraph with no edges leaving it.

## Representations

With n = |V| and m = |E|:

| Representation | Space | Test edge (u, v) | List neighbors |
| :--- | :--- | :--- | :--- |
| Adjacency matrix | O(n²) | O(1) | O(n) |
| Adjacency list | O(n · min(m, n)) | O(min(n, m)) | O(1) |
| Edge list | O(m + n) | O(m) | O(m) |
| Incidence matrix | O(n·m) | O(m) | O(n·m) |

The table follows the slide. The adjacency-list space bound on the slide is loose: storing each edge once per endpoint takes O(n + m), and that tighter bound is what makes BFS and DFS linear. "List neighbors: O(1)" means you can reach the list immediately; iterating over it still costs the vertex's degree.

## Traversal

A generic exploration grows a reached set R from {s} by repeatedly adding any v ∉ R that has an edge from R. Without an order for scanning edges, a naive version costs about O(|E|² log |V|). BFS and DFS choose that order.

- **Breadth-first search (BFS):** visit layer Lᵢ, the nodes at distance i from s, before layer Lᵢ₊₁. Use a queue. The BFS tree gives shortest paths in unweighted graphs.
- **Depth-first search (DFS):** mark s, then recurse into each unvisited neighbor. Use recursion or an explicit stack. DFS finish times power [[concepts/primer/strongly-connected-components|SCCs]] and [[concepts/primer/dags-and-topological-ordering|topological ordering]].

With adjacency lists, both run in O(m + n): each vertex is processed once, and each edge is scanned once per endpoint.

**Pitfall:** BFS and DFS trees make no guarantee about weighted distances. Weighted shortest paths need [[concepts/greedy/dijkstra-shortest-paths|Dijkstra]] or [[concepts/dynamic-programming/bellman-ford|Bellman–Ford]].

Source: `Lecture 11_ Introduction to Graph Algorithms and Traversals.pdf`, PDF pp. 2–59. The representation table was checked visually on PDF p. 34.
