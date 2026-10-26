## Greedy Algorithms and Proof Techniques

A greedy algorithm processes the input in a chosen order and makes each decision as if it were the last one, taking the locally best option. The idea is not fully formalized and is often problem specific; the lecture's definition comes from *priority algorithms*. One problem can have many candidate greedy rules. **Finding** a greedy rule is usually easy. **Proving** it optimal is the real work.

### Warm-up: a Proof by Exchange

Maximize f(x, y) = 12x + 3y over distinct x, y ∈ S = {−1, 2, 5, 8, −3, 7}. Greedy picks x* = max S = 8 and y* = max(S ∖ {x*}) = 7.

*Proof by contradiction:* suppose an optimal (x′, y′) differs from (x*, y*).

- If x′ < y′, swapping them helps, because the larger value should get the larger coefficient. For example f(5, 7) = 81 ≤ f(7, 5) = 99.
- x′ > x* is impossible, because x* is the maximum.
- In every other case, exchanging x′ → x* and y′ → y* does not decrease f.

The pattern is to transform any optimal solution into greedy's solution without ever making it worse.

### Greedy is Not Always Optimal

- **Bin packing, first fit:** boxes of capacity 10 and items 4.9, 4.9, 5.1, 5.1. First fit uses 3 boxes ([4.9, 4.9], [5.1], [5.1]), but 2 boxes suffice ([4.9, 5.1] twice). The general form uses items 1/2 − ε and 1/2 + ε.
- **Coin change with denominations {1, 5, 12, 20} for 24¢:** greedy gives 20 + 1 + 1 + 1 + 1, which is 5 coins. Optimal is 12 + 12, which is 2 coins. The general case needs [[concepts/dynamic-programming/dynamic-programming-method|dynamic programming]].
- **US coins (1, 5, 10, 25, 50¢, $1):** the lecture claims greedy is optimal. For 87¢ it takes 50 + 25 + 10 + 1 + 1, which is 5 coins.

### Two Properties to Look for

1. **Greedy choice property:** there is a simple priority (biggest, smallest, earliest, fastest) under which some optimal solution agrees with the first greedy choice.
2. **Optimal substructure:** after the first choice, the rest is a smaller instance of the same problem, solved optimally by the same rule.

### Technique 1: Greedy Stays ahead

Show by induction that after every step i, greedy's partial solution is at least as good as any other solution's first i steps, measured on a suitable quantity. Optimality then follows at the end. See [[concepts/greedy/interval-scheduling|interval scheduling]] and [[concepts/greedy/dijkstra-shortest-paths|Dijkstra]].

### Technique 2: Exchange Argument

1. **Label the solutions:** A = {a₁, …, aₖ} is greedy's, and O = {o₁, …, oₖ} is an optimal one.
2. **Compare:** if O ≠ A, then either some element is in one and not the other, or two consecutive elements of O appear in the opposite order to A (an *inversion*).
3. **Exchange:** swap the differing element, or the inverted pair, so that O looks more like A, and show it gets no worse.
4. **Conclude:** repeating the swaps turns O into A at equal cost, so A is optimal.

Exchange proofs appear in [[concepts/greedy/minimizing-maximum-lateness|minimizing lateness]], [[concepts/greedy/offline-caching|offline caching]], [[concepts/greedy/huffman-coding|Huffman coding]], and [[concepts/greedy/minimum-spanning-trees|MSTs]].

**Pitfall:** one convincing example is not a proof. Either find a counterexample to the rule or give one of the two arguments.

Source: `Lecture 7_ Greedy Algorithms Easy Level Problems.pdf`, PDF pp. 2–100.
