# Derived Algorithms Solutions

These **derived solutions** are for the [[exam-prep/practice/algorithms-practice|original practice set]]. They are not official answers. Every numeric trace was checked by an independent recomputation.

## 1. Growth rates

(log n)^10 < 2^(lg n) = n < n log n = Θ(log(n!)) < n^1.01 < 1.01ⁿ.

- log(n!) = Θ(n log n) by Stirling's formula.
- Any polylog is o(n).
- n log n = o(n^1.01), because log n = o(n^0.01).
- Any polynomial is o(1.01ⁿ).

See [[review/primer/asymptotic-notation|asymptotic notation]].

## 2. Union–find trace

| Operation | Effect |
| :--- | :--- |
| Union(1,2) | Depths are equal, so parent(1) = 2 and depth(2) = 1 |
| Union(3,4) | parent(3) = 4, depth(4) = 1 |
| Union(2,4) | Roots 2 and 4 have equal depth, so parent(2) = 4 and depth(4) = 2 |
| Union(5,6) | parent(5) = 6, depth(6) = 1 |
| Union(7,8) | parent(7) = 8, depth(8) = 1 |
| Union(6,8) | Equal depths, so parent(6) = 8 and depth(8) = 2 |
| Union(4,8) | Equal depths (2), so parent(4) = 8 and depth(8) = 3 |

The final `parent` array for elements 1..8 is [2, 4, 4, 8, 6, 8, 8, 8]. The root is 8, with depth 3. Find(1) follows 1 → 2 → 4 → 8. The depth bound holds: 8 elements ≥ 2³.

## 3. Heap trace

```text
insert 7: [7]
insert 4: [4, 7]
insert 9: [4, 7, 9]
insert 1: [1, 4, 9, 7]
insert 6: [1, 4, 9, 7, 6]
insert 3: [1, 4, 3, 7, 6, 9]
DeleteMin → 1; move 9 to the root, shift down past 3 → [3, 4, 9, 7, 6]
```

When 1 is inserted it starts at index 3 and swaps with 7 (index 1), then with 4 (index 0).

## 4. Recurrences

- (a) a = b = 3, so n^(log₃3) = n = f(n). Master case 2 gives Θ(n log n).
- (b) n^(log₂4) = n² = f(n). Case 2 gives Θ(n² log n).
- (c) n^(log₄2) = n^(1/2) = f(n). Case 2 gives Θ(√n log n).
- (d) n^(lg 7) ≈ n^2.81 dominates n². Case 1 gives Θ(n^(lg 7)).
- (e) 1/5 + 7/10 = 9/10 < 1, so the Akra–Bazzi exponent p is below 1 and the result is Θ(n).

## 5. Merge-and-count

- [4, 1] splits into [4] and [1]. Merging adds 1 when 1 is taken from the back half. Result [1, 4], 1 inversion.
- [3, 2, 5] splits into [3] and [2, 5].
  - [2, 5] adds 0.
  - Merging [3] with [2, 5] adds 1 when 2 is taken. Result [2, 3, 5], 1 inversion.
- Merging [1, 4] with [2, 3, 5]:
  - take 1;
  - take 2, adding 1 (remaining front: [4]);
  - take 3, adding 1;
  - take 4;
  - take 5, adding 0.
  That adds 2.

The total is 1 + 1 + 2 = 4: the pairs (4,1), (4,3), (4,2), and (3,2).

## 6. Group size

- **Groups of 3:** the pivot beats about half of the n/3 medians and 2 elements in each of their groups, about n/3 elements. The larger side can hold about 2n/3, giving T(n) ≤ T(n/3) + T(2n/3) + cn. The fractions sum to 1, so the result is Θ(n log n), which is not linear.
- **Groups of 7:** the pivot beats about half of the n/7 medians and 4 elements in each of their groups, about 2n/7. The larger side holds at most about 5n/7, giving T(n) ≤ T(n/7) + T(5n/7) + cn. Since 6/7 < 1, this is O(n).

Linear time needs the two fractions to sum to *strictly* less than 1. See [[review/divide-and-conquer/selection-and-median-of-medians|median of medians]].

## 7. Interval scheduling

Sorted by finish time: B4, C5, A6, D7, E8, F9, G10, H11.

- Take B(1,4). Skip C, A, and E, which start before 4.
- Take D(5,7). Skip F and G, which start before 7.
- Take H(8,11).

The selected set is {B, D, H}, of size 3. Brute force confirms 3 is the maximum.

**Counterexample to shortest-first:** (0, 5), (4, 7), (6, 10). Shortest-first takes (4, 7), which overlaps both others, for 1 interval. The optimum is 2.

## 8. Huffman

- Merge e (.10) + c (.15) = .25. Choosing d instead of c on this tie is also valid.
- Merge d (.15) + b (.20) = .35.
- Merge .25 + .35 = .60.
- Merge a (.40) + .60.

One code: a = 0, e = 100, c = 101, d = 110, b = 111.

ABL = 0.40·1 + 0.60·3 = **2.2 bits per symbol**.

## 9. Maximum lateness

The jobs are already in EDF order. Finish times are 3, 5, 6, 10, 13, 15, against deadlines 6, 8, 9, 9, 14, 15. Lateness is 0, 0, 0, 1, 0, 0, so the maximum is **1**. Brute force over all 720 orders confirms this is optimal.

**Swap argument:** suppose i runs just before j and dⱼ < dᵢ. After the swap, j finishes earlier, so its lateness cannot rise. i now finishes at j's old finish time f, and its new lateness f − dᵢ ≤ f − dⱼ, which was j's old lateness. So the maximum does not increase.

## 10. Knapsack table

| Row / capacity | 0 | 1 | 2 | 3 | 4 | 5 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 1 (2, 3) | 0 | 0 | 3 | 3 | 3 | 3 |
| 2 (3, 4) | 0 | 0 | 3 | 4 | 4 | 7 |
| 3 (4, 5) | 0 | 0 | 3 | 4 | 5 | 7 |
| 4 (5, 6) | 0 | 0 | 3 | 4 | 5 | 7 |

The optimum is **7**, using items {1, 2}. Trace back:

- v[4,5] = v[3,5], so skip item 4.
- v[3,5] = v[2,5], so skip item 3.
- v[2,5] ≠ v[1,5], so take item 2 and move to w = 2.
- v[1,2] ≠ v[0,2], so take item 1.

This is the worked table in [[review/dynamic-programming/knapsack-and-subset-sum|knapsack]].

## 11. Edit distance

E[SUNDAY, SATURDAY] = **3**. One optimal alignment:

```text
S--UNDAY
SATURDAY
```

That is 2 insertions (A, T) and 1 substitution (N → R).

## 12. Bellman–Ford

| i | s | a | b | c | d |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 0 | ∞ | ∞ | ∞ | ∞ | 0 |
| 1 | ∞ | ∞ | ∞ | −3 | 0 |
| 2 | ∞ | −1 | 2 | −3 | 0 |
| 3 | 3 | −1 | −2 | −3 | 0 |
| 4 | 0 | −1 | −2 | −3 | 0 |

The shortest path is s → b → a → c → d, with length 0. There is **no** negative cycle: the only cycle, b → a → c → d → b, has weight −1 + 2 − 3 + 4 = 2. A 5th round would change nothing.

## 13. Max flow and min cut

One sequence of augmenting paths: s→a→t (4), s→b→t (6), s→a→b→t (3). That gives v(f) = **13**.

- In the final residual graph, s reaches only a, so ({s, a}, {b, t}) is a min cut with capacity s→b 6 + a→b 3 + a→t 4 = 13.
- **Not unique:** ({s, a, b}, {t}) also has capacity 4 + 9 = 13.

## 14. Reduction design

**Construction:**

- s → each student, capacity 1;
- student → each acceptable project, capacity 1;
- project j → t, capacity cⱼ.

A valid assignment exists exactly when the max flow equals n.

- **Assignment ⇒ flow:** send one unit along s → student → assigned project → t. Project capacities hold because the assignment respects cⱼ.
- **Flow ⇒ assignment:** by integrality, take an integral max flow of value n. Each student edge carries exactly 1, so it leaves along exactly one project edge. Each project receives at most cⱼ units.

**Runtime:** C = n, so Ford–Fulkerson takes O(mn), where m counts the acceptable pairs. See [[review/network-flow/bipartite-matching-and-disjoint-paths|bipartite matching]].

## 15. Project selection

**Network:**

- s → p1 with capacity 10, and s → p2 with capacity 4;
- p3 → t with capacity 6, and p4 → t with capacity 3;
- ∞ edges p1 → p3, p2 → p3, and p2 → p4.

C = 14. The best feasible set is {p1, p2, p3, p4}, with profit 10 + 4 − 6 − 3 = **5**. The min cut is C − 5 = 9, the two sink edges, 6 + 3. For comparison, {p1, p3} earns only 4.

## 16. Independent set and vertex cover

Number the cycle 1–2–3–4–5–1.

- A maximum independent set is {1, 3}, of size 2. Three vertices would include two that are adjacent.
- Its complement {2, 4, 5} is a vertex cover of size 3, and no 2 vertices cover all 5 edges.
- Sizes 2 + 3 = 5 = n, as the complement theorem predicts.

## 17. Clique is NP-complete

1. **In NP:** the certificate is k vertices. Check all C(k, 2) pairs for adjacency in polynomial time.
2. **Known hard problem:** Independent Set.
3. **IS ≤ₚ CLIQUE:** given (G, k), build the complement graph Ḡ, which has an edge exactly where G does not, in O(n²). Ask CLIQUE about (Ḡ, k).
   - If S is independent in G, no two of its vertices are adjacent in G, so all are adjacent in Ḡ: a clique.
   - If S is a clique in Ḡ, it is independent in G.

## 18. Pseudo-polynomial time

The input writes W in about log W bits, so O(nW) is exponential in the input *length* when W is around 2ⁿ. NP-completeness concerns time polynomial in the encoding length. An algorithm running in time polynomial in n and log W would put Subset Sum in P, and then P = NP. See [[review/dynamic-programming/knapsack-and-subset-sum|knapsack]] and [[review/np-completeness/p-np-and-np-completeness|P versus NP]].
