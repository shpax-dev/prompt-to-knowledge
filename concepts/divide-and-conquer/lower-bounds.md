## Lower Bounds

An algorithm gives an **upper bound** on a problem's complexity. A **lower bound** says that *every* algorithm must do at least some amount of work, which tells you when further improvement is futile. To prove one, you must show that certain steps are necessary for any algorithm, not just for the one you wrote.

### Adversary Argument

Imagine an adversary who answers the algorithm's queries so as to force as many steps as possible, while staying consistent with some valid input.

**Search in an unsorted array is Ω(n).** For each position the algorithm inspects, the adversary answers "not x". Re-inspecting a position gains nothing. If the algorithm stops before inspecting every position, the adversary places x in a position that was never checked. So any correct algorithm must, in the worst case, inspect all n positions.

### Decision-tree Argument

Model a comparison-based algorithm as a binary tree:

- Each internal node is one comparison, with two outcomes. A k-way query would give a k-ary tree.
- The input does not change, so repeating a question is useless and the model is a tree, not a graph with cycles.
- Each leaf is an output, and the tree height is the worst-case number of comparisons.

A tree of height k has at most 2^k leaves. If the problem needs M distinct outputs, then 2^k ≥ M, so k ≥ log₂ M. This is the pigeonhole principle: with fewer leaves than outputs, some leaf would have to give two different answers.

| Problem | Distinct outputs M | Lower bound |
| :--- | :--- | :--- |
| Search a sorted array | n + 1 (each position or "absent") | Ω(log n) |
| Comparison sorting | n! orderings | log₂ n! = Ω(n log n) |

**Pitfall:** phrase the argument about *all* algorithms that use the model's queries, not about one specific algorithm's tree.

### Lower Bounds by Reduction

Suppose algorithm A sorts everything except the order of the last two elements. Then A still needs Ω(n log n) comparisons:

- *Counting:* it must distinguish n!/2 outputs, and log(n!/2) = Ω(n log n).
- *Reduction:* if A ran in o(n log n), then "run A, then compare the last two elements" would sort in o(n log n) using comparisons, which is impossible.

### A Tighter Example: 0/1 Arrays

Sorting an array that contains only 0s and 1s:

- One 0 among n − 1 ones has n possible outputs, giving log n. An adversary argument gives Ω(n) because the 0 can hide anywhere.
- With an even split there are C(2n, n) arrangements, and log C(2n, n) = Θ(n) by Stirling's formula.
- A linear-time upper bound comes from the partition step of [[concepts/divide-and-conquer/quicksort-and-its-analysis|quicksort]], so the bound is tight.

A higher lower bound describes the true difficulty better. Ω(1) is always true but says nothing.

**Original practice:** binary search on 1,000 sorted items needs at least ⌈log₂ 1001⌉ = 10 two-way comparisons in the worst case.

Source: `Lecture 4_ Divide and Conquer Strategies Part A.pdf`, PDF pp. 98–153.
