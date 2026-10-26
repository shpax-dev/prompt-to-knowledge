# Polynomial-Time Reductions

- **Easy problems** have polynomial-time algorithms; this is the class P.
- **Hard problems** are ones for which no efficient algorithm is known.

Reductions are how hardness is compared. They are always stated for **decision problems**.

## Decision versus optimization

Optimization asks for the largest matching; the decision version asks whether there is a matching of size ≥ k. They are polynomially equivalent:

- **Optimization → decision:** solve the optimization version and compare its value with k.
- **Decision → optimization:** the answer lies between 0 and an upper bound N, such as min(|A|, |B|) for matching. Ask for k = N, N − 1, … until you get "yes", or [[review/divide-and-conquer/binary-search-on-the-answer|binary-search]] the range.

## Definition: Y ≤ₚ X

"Y reduces to X in polynomial time" means that any instance of Y can be solved by transforming it in polynomial time into polynomially many instances of X, calling a black-box solver for X on each, and combining the answers efficiently.

- If **X** is solvable in polynomial time, then so is **Y**.
- If **Y** cannot be solved in polynomial time, then neither can **X**. X is at least as hard as Y.

**Direction pitfall:** to show that a new problem X is hard, reduce a *known hard* problem Y **to** X. Assume a solver for X, and use it to solve Y.

Reductions are transitive: Z ≤ₚ Y and Y ≤ₚ X imply Z ≤ₚ X.

## Packing versus covering: Independent Set and Vertex Cover

- **Independent Set (IS):** is there an S ⊆ V with |S| ≥ k and no two nodes of S adjacent? This is a *packing* problem.
- **Vertex Cover (VC):** is there an S ⊆ V with |S| ≤ k touching every edge? This is a *covering* problem.

**Theorem:** S is independent if and only if V − S is a vertex cover.

- If S is independent, every edge has at most one end in S, so at least one end in V − S.
- If V − S is a cover, an edge with both ends in S would be uncovered.

So IS ≤ₚ VC and VC ≤ₚ IS, with k ↔ n − k.

## Reductions to set problems

- **Set Cover (SC):** given a universe U, subsets S₁, …, Sₘ, and k, do at most k subsets cover U?
  - **VC ≤ₚ SC:** set U = E, and for each vertex v make the set of edges incident to v. A vertex cover of size ≤ k corresponds to a set cover of size ≤ k.
- **Set Packing (SP):** are there at least k pairwise disjoint subsets?
  - **IS ≤ₚ SP:** the same construction. Independent vertices have disjoint edge sets.

## Gadgets: 3SAT ≤ₚ IS

**3SAT:** a conjunction of clauses, each an OR of exactly 3 literals. Is there a satisfying truth assignment?

A **gadget** is a piece of X that simulates one component of Y.

- **Clause gadget:** a triangle, one node per literal of the clause.
- **Conflict edges:** join every occurrence of xᵢ to every occurrence of ¬xᵢ.
- **Target:** k = the number of clauses.

An independent set of size k picks exactly one node per triangle, a literal made true. Conflict edges stop xᵢ and ¬xᵢ from both being chosen. So the formula is satisfiable exactly when the graph has an independent set of size ≥ k.

## The chains so far

```text
3SAT ≤ₚ IS ≤ₚ VC ≤ₚ SC
3SAT ≤ₚ IS ≤ₚ SP
VC   ≤ₚ IS ≤ₚ SP
```

Once 3SAT is shown NP-complete in [[review/np-completeness/p-np-and-np-completeness|P, NP, and NP-completeness]], every problem in these chains is NP-complete.

Source: `Lecture 15_ NP Reductions and Intractability.pdf`, PDF pp. 2–58.
