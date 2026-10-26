# Interval Scheduling

Each request rᵢ = (sᵢ, fᵢ) has a start time and a finish time. Two requests are **compatible** if one finishes no later than the other starts: fᵢ ≤ sⱼ or fⱼ ≤ sᵢ. The goal is a compatible set of maximum size.

## Which greedy rule?

| Heuristic | Optimal? |
| :--- | :--- |
| Earliest start time | No |
| Shortest interval | No |
| Fewest conflicts | No (the lecture's counterexample on PDF pp. 113–115 is a figure) |
| **Earliest finish time** | **Yes** |

**Derived counterexamples:**

- *Earliest start:* (0, 10), (1, 2), (3, 4). Greedy takes only (0, 10), but the optimum has 2.
- *Shortest:* (0, 5), (4, 7), (6, 10). Greedy takes (4, 7), which overlaps both others, but the optimum is (0, 5) and (6, 10).

## Finish-First

```text
S ← ∅
while requests remain:
  pick r with the smallest finish time; add r to S
  remove every request incompatible with r
```

**Proof (greedy stays ahead):**

- Let S = ⟨i₁, …, iₖ⟩ and an optimal S* = ⟨j₁, …, jₘ⟩, each sorted by finish time.
- **Lemma:** f(iᵣ) ≤ f(jᵣ) for every r ≤ k.
  - *Base case:* Finish-First starts with the earliest finish time of all.
  - *Inductive step:* f(iᵣ₋₁) ≤ f(jᵣ₋₁) ≤ s(jᵣ), so jᵣ was still available when greedy chose iᵣ. Greedy picks the smallest finish time, so f(iᵣ) ≤ f(jᵣ).
- **Theorem:** suppose m > k. Then j₍ₖ₊₁₎ starts after f(jₖ) ≥ f(iₖ), so it was still compatible when greedy stopped, which contradicts greedy stopping. So k = m.

There can be several optimal schedules, so the claim proved is |S| = |S*|, not S = S*.

## Implementation

Sort by finish time in O(n log n). Then scan once, keeping a request whenever its start time is at least the last kept finish time, in O(n). The total is O(n log n).

**Derived trace:** a(1,4), b(3,5), c(0,6), d(5,7), e(3,9), f(5,9), g(6,10), h(8,11). By finish time, keep a. b, c, and e start before 4, so skip them. Keep d. f and g start before 7, so skip them. Keep h. The result is {a, d, h}, of size 3.

## Variants

- **Online:** requests arrive one at a time, without knowledge of the rest.
- **Weighted:** each interval has a value. Greedy fails, so use [[review/dynamic-programming/weighted-interval-scheduling|dynamic programming]].
- **Interval coloring:** schedule *all* requests on as few resources as possible.

Source: `Lecture 7_ Greedy Algorithms Easy Level Problems.pdf`, PDF pp. 101–145.
