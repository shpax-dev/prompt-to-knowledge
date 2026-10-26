## Bellman–Ford Shortest Paths

Input: a directed graph with n nodes, m edges, and edge weights cᵥw that may be **negative**, but no negative cycles. Goal: shortest paths.

### Why Negative Cycles Are Excluded

If a negative cycle lies on a path from s to t, you can loop around it forever, so the shortest path length is −∞. Without negative cycles, some shortest path is **simple** and has at most n − 1 edges.

### The DP

The lecture computes shortest paths *to* a fixed sink t.

**Matrix:** M[i][v] is the length of the shortest v → t path that uses at most i edges.

**Bellman equation:** the best path either uses at most i − 1 edges, or it starts with some edge v → w.

```text
M[0][t] = 0,   M[0][v] = ∞ for v ≠ t
M[i][v] = min( M[i−1][v],  min over w of ( M[i−1][w] + cᵥw ) )     # cᵥw = ∞ if there is no edge
```

**Solution:** M[n − 1][s].

**Runtime:**

- O(n²) cells at O(n) each is O(n³).
- Checking only each node's outgoing edges gives O(n · ∑ deg(v)) = O(mn).

**Space:** keep a single array M[v] and update it in place each round, for O(n). To recover the path, keep `first[v]`, the next hop on v's current best path.

### Negative-cycle Detection

If there is no negative cycle, M[i][v] = M[n − 1][v] for every i ≥ n − 1.

**Test:** add a node t with a 0-cost edge from every node, run n rounds instead of n − 1, and check whether M[n][v] ≠ M[n − 1][v] for any v. A change means a negative cycle exists.

### Derived Trace (Shortest Paths to T = D)

Edges: s→a 4, s→b 2, b→a −1, a→c 2, b→c 5, c→d −3, d→b 4. The only cycle, b→a→c→d→b, has weight +2.

| i | s | a | b | c | d |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 0 | ∞ | ∞ | ∞ | ∞ | 0 |
| 1 | ∞ | ∞ | ∞ | −3 | 0 |
| 2 | ∞ | −1 | 2 | −3 | 0 |
| 3 | 3 | −1 | −2 | −3 | 0 |
| 4 | 0 | −1 | −2 | −3 | 0 |

The shortest s → d path is s → b → a → c → d, with length 2 − 1 + 2 − 3 = 0.

**Pitfall:** one entry staying the same for a round does not make it final; M[i][s] drops from 3 to 0 in the last round above. *Derived note:* you may stop early only when an *entire* row repeats the previous row.

[[concepts/greedy/dijkstra-shortest-paths|Dijkstra]] is faster but needs non-negative weights. Bellman–Ford also finds the cheapest augmenting paths in min-cost flow; see [[concepts/network-flow/max-flow-and-ford-fulkerson|max flow]].

Source: `Lecture 12_ Shortest Paths and Minimum Spanning Trees.pdf`, PDF pp. 26–57.
