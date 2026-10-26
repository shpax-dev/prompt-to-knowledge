# All-Pairs Shortest Paths

The goal is the distance d(v, u), and a shortest path, for every ordered pair of vertices. Store the distances in an n × n matrix D, and the paths in predecessor matrices.

## Options

| Approach | Weights | Time |
| :--- | :--- | :--- |
| Run Bellman–Ford from every vertex | May be negative | Θ(n²m) |
| Run Dijkstra from every vertex | Non-negative | Θ(nm + n² log n) |
| Floyd–Warshall | May be negative, no negative cycles | Θ(n³) |
| Johnson (reweight, then n × Dijkstra) | May be negative, sparse graphs | Θ(nm + n² log n) |

The single-source building blocks are [[concepts/dynamic-programming/bellman-ford|Bellman–Ford]] and [[concepts/greedy/dijkstra-shortest-paths|Dijkstra]]. For a dense graph, m = Θ(n²), so n × Dijkstra costs O(n³) anyway. When m = o(n²), n × Dijkstra beats Θ(n³).

## Floyd–Warshall

Number the vertices v₁, …, vₙ. Let dₖ(i, j) be the shortest i → j distance using only v₁, …, vₖ as *intermediate* vertices.

```text
d₀(i, j) = 0 if i = j;  w(i, j) if there is an edge;  ∞ otherwise
dₖ(i, j) = min( dₖ₋₁(i, j),  dₖ₋₁(i, k) + dₖ₋₁(k, j) )
```

That is n rounds over n² pairs, so Θ(n³). It uses an adjacency matrix for quick access to edge weights.

**Negative cycles:** if any diagonal entry d(i, i) becomes negative, there is a negative cycle through i.

**Source example:** edges A→B 3, A→C 2, B→D 7, C→E 4, E→C 5, E→B 4, D→E 6. The final matrix, rows "from" and columns "to":

```text
     A    B    C    D    E
A    0    3    2   10    6
B  INF    0   18    7   13
C  INF    8    0   15    4
D  INF   10   11    0    6
E  INF    4    5   11    0
```

For example, B→C = 18 via B→D→E→C. A derived recomputation reproduces every entry.

## Johnson's algorithm

1. Add a new vertex s with a 0-weight edge to every vertex.
2. Run Bellman–Ford from s to get h(v) = d(s, v), and stop if there is a negative cycle.
3. Reweight every edge to be non-negative, then run Dijkstra from every vertex.

**Unresolved:** the slide on PDF p. 40 prints the reweighting as ŵ(u, v) = w(v, u) + h(v) − h(u). The usual textbook form is ŵ(u, v) = w(u, v) + h(u) − h(v). That form is non-negative because h(v) ≤ h(u) + w(u, v), and it adds h(u) − h(v) to *every* u–v path, so shortest paths do not change. Confirm which form the course intends before relying on the slide's version.

## Single-source summary

| Setting | Algorithm | Time |
| :--- | :--- | :--- |
| Negative weights | Bellman–Ford | Θ(nm) |
| DAG, negative weights allowed | Topological-order DP | Θ(m + n) |
| Non-negative weights | Dijkstra | Θ(m + n log n) |

The DAG method is in [[concepts/primer/dags-and-topological-ordering|topological ordering]].

Adding a large constant to make weights positive does **not** preserve shortest paths, because paths with more edges are penalized more.

Source: `Lecture 13_ All Pair Shortest Paths.pdf`, PDF pp. 2–41. PDF pp. 6–22 are mostly images; the example table was checked visually on PDF p. 22, and the Johnson formula on PDF p. 40.
