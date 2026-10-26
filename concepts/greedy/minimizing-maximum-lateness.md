# Minimizing Maximum Lateness

There are n jobs and one machine that runs one job at a time. Job i has processing time tᵢ and deadline dᵢ. If job i finishes at time fᵢ, its lateness is lᵢ = max(0, fᵢ − dᵢ). The goal is to schedule **all** jobs to minimize the maximum lateness.

## Which greedy rule?

| Heuristic | Counterexample, as (tᵢ, dᵢ) |
| :--- | :--- |
| Shortest processing time first | (1, 100), (10, 10): it gives max lateness 1, while running (10, 10) first gives 0 |
| Smallest slack dᵢ − tᵢ first | (1, 2), (10, 10): it runs (10, 10) first for lateness 9, while the other order gives 1 |
| **Earliest deadline first (EDF)** | Optimal |

EDF ignores the processing times entirely.

## Optimality by exchange

1. **No idle time:** some optimal schedule has no gaps, and EDF has none.
2. **Inversion:** a pair where job i runs before job j but dⱼ < dᵢ.
3. **Lemma:** all schedules with no inversions and no idle time have the same maximum lateness. They differ only in the order of jobs with equal deadlines. Those jobs run consecutively, and the last of them has the same finish time in every such order.
4. **Theorem:** some optimal schedule has no inversions and no idle time.
   - If an optimal S* has an inversion, it has one between two *adjacent* jobs i and then j. Swap them.
   - j's lateness can only decrease. i now finishes at j's old finish time, so l′ᵢ = f*ⱼ − dᵢ ≤ f*ⱼ − dⱼ = l*ⱼ.
   - The maximum does not increase, and the number of inversions drops by one. Repeat.
5. **Conclusion:** EDF has no inversions and no idle time, so by the lemma it matches the optimal schedule from step 4.

**Runtime:** sort by deadline, O(n log n).

**Derived trace:** jobs (t, d) = (3,6), (2,8), (1,9), (4,9), (3,14), (2,15) are already in EDF order. Finish times are 3, 5, 6, 10, 13, 15. Latenesses are 0, 0, 0, 1, 0, 0, so the maximum is 1. Brute force over all 720 orders confirms 1 is optimal.

**Pitfall:** with several optimal schedules, "|S| = |S*|" is meaningless here. You must show equal *maximum lateness*.

Compare the stays-ahead proof for [[concepts/greedy/interval-scheduling|interval scheduling]], and the general recipe in [[concepts/greedy/greedy-proof-techniques|greedy proof techniques]].

Source: `Lecture 8_ Greedy Algorithms Hard Level Problems.pdf`, PDF pp. 87–130.
