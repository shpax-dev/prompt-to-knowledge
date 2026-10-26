# Classic NP-Complete Reductions

Each proof below follows the three-step recipe in [[concepts/np-completeness/p-np-and-np-completeness|P, NP, and NP-completeness]]: membership in NP, a known hard source problem, and a reduction proved in both directions.

## A taxonomy of hard problems

| Family | Problems |
| :--- | :--- |
| Packing | Independent Set, Set Packing, Clique |
| Covering | Vertex Cover, Set Cover |
| Sequencing | Hamiltonian Cycle, Hamiltonian Path, TSP |
| Partitioning | 3-D Matching, Graph Coloring |
| Numerical | Subset Sum, Knapsack |
| Constraint satisfaction | 3SAT, Circuit SAT, NAE-4SAT |

When a new problem resembles a family, try a reduction from that family first.

## Sequencing

- **Hamiltonian Cycle:** does a digraph have a cycle that visits every node exactly once?
  - *In NP:* the certificate is the order in which nodes are visited.
  - *3SAT ≤ₚ HamCycle:* each variable xᵢ becomes a row Pᵢ of 3k + 2 nodes, where k is the number of clauses. Traversing the row left to right means xᵢ = 1; right to left means xᵢ = 0. Each clause gets a node that can be visited as a detour from a row, but only in the direction that makes one of its literals true.
- **TSP (decision):** is there a tour of length ≤ D?
  - *HamCycle ≤ₚ TSP:* make one city per vertex. Distance 1 if (u, v) ∈ E, otherwise 2. Set the bound D = n. A Hamiltonian cycle gives a tour of length n. A tour of length n uses no distance-2 pair, so it is a Hamiltonian cycle.
- **Hamiltonian Path:**
  - *HamCycle ≤ₚ HamPath:* split one vertex v into v′, which keeps v's outgoing edges, and v″, which keeps its incoming edges. A Hamiltonian path from v′ to v″ is exactly a Hamiltonian cycle through v.

## Partitioning

- **3-D Matching:** given disjoint sets X, Y, Z of size n and triples T ⊆ X × Y × Z, can n triples cover every element exactly once?
  - *3SAT ≤ₚ 3DM:* each variable gets a ring gadget of 2k core elements and 2k tips. Choosing the even or odd triples sets the variable to true or false. Each clause gets two elements that must take a free tip from a satisfying literal. (n − 1)k clean-up gadgets absorb the leftover tips.
- **3-Coloring:** can the nodes be colored with 3 colors so that neighbors differ?
  - *3SAT ≤ₚ 3-Coloring:* a base triangle T–F–B fixes the palette. Each variable gets nodes vᵢ and ¬vᵢ, joined to each other and both joined to B, so one is colored True and the other False. A clause gadget, shown in a figure on PDF p. 167, can be colored only if at least one of its literals is colored True.

## Numerical

- **Subset Sum:** do some of the numbers w₁, …, wₙ add up to exactly W?
  - The O(nW) [[concepts/dynamic-programming/knapsack-and-subset-sum|DP]] is only pseudo-polynomial.
  - *3DM ≤ₚ Subset Sum:* treat each triple (i, j, k) as a 3n-digit number with 1s in positions i, n + j, and 2n + k. Use base m + 1 so additions never carry. The target W has a 1 in every position.

## Constraint satisfaction

- **NAE-4SAT:** is there an assignment under which every clause has at least one true literal *and* at least one false literal?
  - *3SAT ≤ₚ NAE-4SAT:* add the same new literal v to every clause.
  - If the 3SAT formula is satisfiable, set v = 0.
  - Conversely, if v = 0 in an NAE solution, each clause has another true literal. If v = 1, flip every variable: the complement of an NAE solution is also an NAE solution, and now v = 0.

**Pitfall:** reductions run from the *known hard* problem to the *new* problem. A reduction in the other direction proves nothing about hardness. Building the instance is only half the work; you also have to prove both directions.

Source: `Lecture 15_ NP Reductions and Intractability.pdf`, PDF pp. 118–193. The gadget figures on PDF pp. 124–125, 148–153, and 162–167 are diagrams.
