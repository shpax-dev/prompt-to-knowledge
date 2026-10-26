## Union–Find

Union–find (disjoint-set) maintains a partition of elements into groups. It supports three operations:

- **Init(S):** make a singleton set {x} for each x ∈ S.
- **Union(x, y):** merge the sets containing x and y.
- **Find(x):** return the *leader* (representative) of x's set. Two elements are in the same set exactly when Find returns the same leader for both.

### Reversed Trees

Each set is a tree whose edges point **up** to a parent. The leader is the root, and the root is its own parent. Store the structure as an array `parent[]`:

```text
Find(x):  while x ≠ parent(x): x ← parent(x); return x
Union(x, y):  x ← Find(x); y ← Find(y); link one root under the other
```

Linking costs O(1) once the roots are known. Find costs the height of the tree. If every set is a chain, Find is Θ(n).

**Source example:** the array `parent = [7,5,7,8,8,7,5,7,8,8]` for elements 0–9 has two roots, 7 and 8. Find(1) follows 1 → 5 → 7, and Find(3) follows 3 → 8.

### Union by Depth (Rank)

Always make the root of the **shallower** tree a child of the deeper root. On a tie, pick either root as the new root and increase its depth by one.

**Theorem:** a leader with depth d has at least 2^d elements in its tree.

*Proof by induction:* a depth-0 leader has 1 = 2⁰ element. A leader first reaches depth d only when two trees with depth d − 1 merge, and each has at least 2^(d−1) elements.

With n elements, every depth is therefore at most log₂ n, so Find and Union run in O(log n) worst case.

### Path Compression

During Find, point every node on the search path directly at the root:

```text
Find(x):  if x ≠ parent(x): parent(x) ← Find(parent(x))
          return parent(x)
```

Together with union by rank, this makes Find nearly constant amortized time. The lecture says O(α(n)) and leaves the full analysis to the discussion section.

**Pitfall:** the O(log n) bound depends on balanced unions. Linking an arbitrary root under another can still build a chain.

[[concepts/greedy/minimum-spanning-trees|Kruskal's algorithm]] uses union–find to detect cycles: it makes 2m Finds and n − 1 Unions.

Source: `Lecture 3_ Advanced Data Structures Union-Find and Heaps.pdf`, PDF pp. 2–38. Union by rank also appears in `Lecture 12_ Shortest Paths and Minimum Spanning Trees.pdf`, PDF pp. 112–124.
