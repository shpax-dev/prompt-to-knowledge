# NP-Completeness Preparation

This guide covers Lecture 15. Almost every NP-completeness question uses the same three-step template. Most lost points come from the direction of the reduction, or from proving only one direction of its correctness.

## The template

To prove that X is NP-complete:

1. **X ∈ NP:** name the certificate and give a polynomial-time check.
2. **Pick a known NP-complete problem Y** from the same family in the [[concepts/np-completeness/classic-np-complete-reductions|taxonomy]].
3. **Prove Y ≤ₚ X:**
   - give the construction, from an arbitrary instance of Y to an instance of X, in polynomial time;
   - prove that a yes-instance of Y gives a yes-instance of X;
   - prove that a yes-instance of X means the Y instance was yes.

## Direction check

Say it aloud: "If I could solve **X** quickly, I could solve **Y** quickly." Y is the known hard problem. If your sentence names the new problem as Y, the reduction is backwards. See [[concepts/np-completeness/polynomial-time-reductions|polynomial-time reductions]].

## Family cheat sheet

| New problem looks like… | Try reducing from |
| :--- | :--- |
| Choose many items with no conflicts | Independent Set |
| Cover everything cheaply | Vertex Cover, Set Cover |
| Visit everything in an order | Hamiltonian Cycle or Path |
| Assign to one of k groups | 3-Coloring, 3-D Matching |
| Hit an exact numeric target | Subset Sum |
| Boolean constraints | 3SAT |

## Concepts to state precisely

- A certifier saying "no" for one t does not prove a no-instance.
- NP-hard does not mean in NP. NP-complete means both.
- An O(nW) algorithm does not show P = NP, because it is pseudo-polynomial. See [[concepts/dynamic-programming/knapsack-and-subset-sum|knapsack]].
- If NP ≠ coNP, then P ≠ NP. Know the contrapositive proof in [[concepts/np-completeness/p-np-and-np-completeness|P, NP, and NP-completeness]].

Use [[exam-prep/practice/algorithms-practice|original practice questions]] 16–18.

Source: Lecture 15. See [[setup/source-materials|source materials]].
