# Min-Cut Applications

In these problems the answer is a **partition**, read off a minimum cut. The trick is to arrange the capacities so that each cut's capacity equals the cost you want to minimize.

## Image segmentation

Each pixel i has a foreground likelihood aᵢ > 0 and a background likelihood bᵢ > 0. Each pair of neighboring pixels i, j has a separation penalty pᵢⱼ, paid if exactly one of them is in the foreground set A.

- **Goal:** maximize q(A, B) = ∑ over A of aᵢ + ∑ over B of bⱼ − ∑ over separated neighbor pairs of pᵢⱼ.
- **Rewrite:** with Q = ∑ (aᵢ + bᵢ), maximizing q is the same as *minimizing* ∑ over B of aᵢ + ∑ over A of bⱼ + ∑ over separated pairs of pᵢⱼ.
- **Network:**
  - one node per pixel;
  - edges in both directions between neighbors, with capacity pᵢⱼ;
  - s → i with capacity aᵢ;
  - i → t with capacity bᵢ.
- **Why the min cut is right:** a pixel in A contributes bᵢ to the cut, a pixel in B contributes aᵢ, and each separated neighbor pair contributes pᵢⱼ. That is exactly the expression being minimized.

## Project selection

Projects have profits pᵢ, which may be negative. A precedence graph says project i requires project j. Choose a feasible set A, one closed under requirements, that maximizes profit(A) = ∑ over A of pᵢ.

- **Network:**
  - s → i with capacity pᵢ for each pᵢ > 0;
  - i → t with capacity −pᵢ for each pᵢ < 0;
  - precedence edges i → j with capacity ∞ (or C + 1).
- **Bound:** with C = ∑ of positive pᵢ, the cut ({s}, everything else) has capacity C, so the min cut is at most C. Infinite edges therefore never cross a minimum cut, and the source side of a min cut is a feasible set.
- **Key identity:** c(A′, B′) = C − profit(A). Minimizing the cut maximizes the profit.

**Derived example:** p1 = +10, p2 = +4, p3 = −6, p4 = −3. p1 requires p3; p2 requires p3 and p4. Here C = 14. The best feasible set is all four projects, with profit 5, and a max-flow computation confirms the min cut is 14 − 5 = 9.

## Baseball elimination

Each team x has wₓ wins so far, and each pair x, y has gₓᵧ games left against each other. Has team z been eliminated?

**Source example:** New York 92, Toronto 91, Baltimore 91, Boston 90. The remaining games are NYY–TOR, TOR–BAL, BAL–BOS, BOS–TOR, NYY–BAL. Boston can reach at most 92 wins. The other three teams have 274 wins plus 3 games among themselves, so 277 wins in total, an average of 92⅓. One of them must reach 93, so Boston is eliminated.

**Network:** let m be z's maximum possible wins and S′ the other teams.

- s → (game node for x, y) with capacity gₓᵧ;
- each game node → both of its team nodes, with capacity ∞;
- team x → t with capacity m − wₓ.

z survives exactly when the max flow saturates every game edge, that is v(f) = g* = ∑ gₓᵧ.

**Certificate:** if z is eliminated, the min cut yields a set T ⊆ S′ with m·|T| < ∑ over x ∈ T of wₓ + ∑ over x, y ∈ T of gₓᵧ. In other words, the teams in T must average more than m wins.

**Pitfall:** the capacity m − wₓ must not be negative. If some wₓ > m already, z is trivially eliminated.

Source: `Lecture 14_ Network Flow and Combinatorial Optimization.pdf`, PDF pp. 108–124, 180–194, and 205–231.
