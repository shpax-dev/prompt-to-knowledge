## DAGs and Topological Ordering

A **directed acyclic graph (DAG)** has no directed cycles. DAGs model precedence, such as getting dressed: socks come before shoes.

A **topological ordering** lists the nodes so that every edge (u, v) has u before v.

### Facts

- **Observation:** if G has a topological ordering, then G is a DAG. A cycle would need some edge to point backward.
- **Key property:** every DAG has a node with no incoming edges.
  - *Proof by contradiction:* suppose every node has an incoming edge. Walk backward along incoming edges. After |V| + 1 steps, some node repeats by the pigeonhole principle, which gives a cycle.
- **Theorem:** every DAG has a topological ordering.
  - *Proof by induction on |V|:* removing a node that respects the key property leaves a smaller DAG, which has an ordering by the induction hypothesis.

### Algorithm from the Proof

The inductive proof is an algorithm: repeatedly remove a node with no remaining incoming edges and output it next. The lecture's visualization builds the order from the other end, placing nodes at the back; the reasoning is symmetric.

*Derived implementation:* keep an in-degree counter for each node and a list of the nodes whose count is 0. Removing a node decrements its out-neighbors' counters. This takes O(m + n). If nodes remain but none has in-degree 0, the graph has a cycle.

### Why DAGs Matter Later

A topological order is the evaluation order for [[concepts/dynamic-programming/dynamic-programming-method|dynamic programming]]: subproblems are nodes, dependencies are edges. Shortest paths in a DAG take one pass in topological order, even with negative weights, in Θ(m + n). See [[concepts/dynamic-programming/all-pairs-shortest-paths|the shortest-path summary]].

Source: `Lecture 11_ Introduction to Graph Algorithms and Traversals.pdf`, PDF pp. 122–152. DAG shortest paths: `Lecture 9_ Dynamic Programming A Primer.pdf`, PDF pp. 56–74.
