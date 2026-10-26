# Original Algorithms Practice

These are **original practice** questions written for this vault. They are inspired by the mechanisms in the supplied lectures; they do not reproduce lecture or exam questions, and they are not an official exam. State your assumptions and show intermediate work.

The separate [[exam-prep/practice/algorithms-solutions|derived solutions]] contain the answers. In quiz mode, present one question at a time without opening or quoting a solution.

Each section has a matching study guide: [[exam-prep/study-guides/primer-preparation|primer]], [[exam-prep/study-guides/divide-and-conquer-preparation|divide and conquer]], [[exam-prep/study-guides/greedy-preparation|greedy]], [[exam-prep/study-guides/dynamic-programming-preparation|dynamic programming]], [[exam-prep/study-guides/network-flow-preparation|network flow]], and [[exam-prep/study-guides/np-completeness-preparation|NP-completeness]].

## Primer

### 1. Growth rates

Arrange these in increasing asymptotic order, and mark any that are Θ of each other: n^1.01, (log n)^10, log(n!), 2^(lg n), n log n, 1.01ⁿ.

### 2. Union–find trace

Elements 1–8 start as singletons. Use union by depth with the lecture's rule:

- find both roots x = Find(a) and y = Find(b);
- if depth(x) > depth(y), set parent(y) ← x;
- otherwise set parent(x) ← y, and if the depths were equal, increase depth(y).

Apply, in order: Union(1,2), Union(3,4), Union(2,4), Union(5,6), Union(7,8), Union(6,8), Union(4,8). Give the final `parent` array, the root's depth, and the path Find(1) follows. No path compression.

### 3. Heap trace

Insert 7, 4, 9, 1, 6, 3 into an empty binary min-heap stored in a 0-based array. Give the array after each insert, then after one DeleteMin.

## Divide and conquer

### 4. Recurrences

Solve each recurrence asymptotically and name the method:

- (a) T(n) = 3T(n/3) + n
- (b) T(n) = 4T(n/2) + n²
- (c) T(n) = 2T(n/4) + √n
- (d) T(n) = 7T(n/2) + n²
- (e) T(n) = T(n/5) + T(7n/10) + n

### 5. Merge-and-count

Count the inversions in [4, 1, 3, 2, 5] using sort-and-count. The front half is the first ⌊n/2⌋ elements. Show how much each merge adds.

### 6. Group size in median of medians

Median of medians uses groups of 5. What recurrence do you get with groups of 3? With groups of 7? Which ones give linear time, and why?

## Greedy

### 7. Interval scheduling

Requests (s, f): A(0,6), B(1,4), C(3,5), D(5,7), E(3,8), F(5,9), G(6,10), H(8,11). Run Finish-First and give the selected set. Then give a 3-interval counterexample showing that "shortest interval first" is not optimal.

### 8. Huffman

Frequencies: a 0.40, b 0.20, c 0.15, d 0.15, e 0.10. Build a Huffman tree, give one valid code, and compute the ABL.

### 9. Maximum lateness

Jobs (tᵢ, dᵢ): (3,6), (2,8), (1,9), (4,9), (3,14), (2,15). Give the EDF schedule's finish times and its maximum lateness. Then explain in two sentences why swapping an adjacent inverted pair cannot increase the maximum lateness.

## Dynamic programming

### 10. Knapsack table

W = 5. Items (weight, value): 1 = (2, 3), 2 = (3, 4), 3 = (4, 5), 4 = (5, 6). Fill the table v[i, w] and recover an optimal item set.

### 11. Edit distance

Compute the edit distance from SUNDAY to SATURDAY using insertions, deletions, and substitutions of cost 1. Give the final value and one optimal alignment.

### 12. Bellman–Ford

Directed edges: s→a 4, s→b 2, b→a −1, a→c 2, b→c 5, c→d −3, d→b 4. Using the lecture's formulation M[i][v], the shortest v → t path with at most i edges where t = d, fill the rows for i = 0..4. Is there a negative cycle?

## Network flow

### 13. Max flow and min cut

Capacities: s→a 8, s→b 6, a→b 3, a→t 4, b→t 9. Find a maximum flow with Ford–Fulkerson, listing each augmenting path and its bottleneck. Give a minimum cut. Is it unique?

### 14. Reduction design

Each of n students lists the projects they would accept. Project j can take at most cⱼ students, and every student must get exactly one project. Design a flow network that decides whether a valid assignment exists, and argue both directions of correctness.

### 15. Project selection

Profits: p1 = +10, p2 = +4, p3 = −6, p4 = −3. p1 requires p3, and p2 requires p3 and p4. Build the min-cut network, give C, and find the optimal project set and its profit.

## NP-completeness

### 16. Independent set and vertex cover

In the 5-cycle C₅, find a maximum independent set and a minimum vertex cover. Verify the complement relationship.

### 17. Clique

CLIQUE asks: does G contain k mutually adjacent vertices? Prove CLIQUE is NP-complete using the lecture's three-step template.

### 18. Pseudo-polynomial time

Subset Sum has an O(nW) algorithm and is NP-complete. Explain why this is not a contradiction, and what would have to change for it to imply P = NP.
