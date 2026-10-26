# Strongly Connected Components

In a directed graph, nodes u and v are **mutually reachable** if there is a path from u to v and a path from v to u. Mutual reachability is transitive. A digraph is *strongly connected* if every pair of nodes is mutually reachable.

A **strongly connected component (SCC)** is a maximal strongly connected subgraph. Every digraph splits into SCCs.

## Testing one pair

Run [[review/primer/graph-basics-and-traversal|BFS or DFS]] from u in G to check that u reaches v. Then run a search from u in G_rev, the graph with every edge reversed, to check that v reaches u.

## Kosaraju's algorithm

1. Run DFS on G, pushing each node onto a stack S when it finishes.
2. Build G_rev and mark every node unvisited.
3. While S is not empty, pop v. If v is unvisited, run DFS from v in G_rev. Every node reached is one SCC.

To label the components, keep an array `SCC[]` initialized to −1 and write the component number into it as each component is finalized.

Runtime: two DFS passes and one reversal, so O(m + n).

## Why it works

- **Key lemma:** let C be an SCC and v a vertex outside C. If some vertex of C has a path to v, then the largest finish time in C is greater than f[v].
- **Corollary:** if there is a path from SCC C₁ to SCC C₂, then max finish time in C₁ > max finish time in C₂.
- **Consequence:** the component with the largest remaining finish time has no edges *into* it from unprocessed components in G. In G_rev it therefore has no edges *out* to them, so DFS in G_rev from its top node stays inside exactly that component.

**Pitfall:** the second pass must run on the reversed graph and in decreasing finish-time order. Changing either one merges components.

Contracting every SCC to a single node leaves a DAG; see [[review/primer/dags-and-topological-ordering|topological ordering]].

Source: `Lecture 11_ Introduction to Graph Algorithms and Traversals.pdf`, PDF pp. 60–121. The step-by-step execution on PDF pp. 73–115 is mostly diagrams.
