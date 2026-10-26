# Asymptotic Notation

Asymptotic notation compares how functions grow as n → ∞. It ignores constant factors and small inputs.

## Definitions

| Notation | Meaning | Formal condition |
| :--- | :--- | :--- |
| f ∈ O(g) | f grows no faster than g (≤) | ∃ c, n₀ > 0 with 0 ≤ f(n) ≤ c·g(n) for all n ≥ n₀ |
| f ∈ Ω(g) | f grows at least as fast as g (≥) | ∃ c, n₀ > 0 with 0 ≤ c·g(n) ≤ f(n) for all n ≥ n₀ |
| f ∈ Θ(g) | same growth rate (=) | ∃ c₁, c₂, n₀ > 0 with c₁·g(n) ≤ f(n) ≤ c₂·g(n) for all n ≥ n₀ |
| f ∈ o(g) | strictly slower (<) | for every c > 0 there is an n₀ with f(n) < c·g(n); equivalently f/g → 0 |
| f ∈ ω(g) | strictly faster (>) | for every c > 0 there is an n₀ with c·g(n) < f(n); equivalently f/g → ∞ |

Key property: f ∈ Θ(g) if and only if f ∈ O(g) and f ∈ Ω(g). Writing f = O(g) is common but is technically an abuse of notation for f ∈ O(g).

## Worked example: insertion sort

Insertion sort's worst case is T(n) = an² + bn − d for constants a > 0, b, d. Then:

- T ∈ Θ(n²), so also T ∈ O(n²) and T ∈ Ω(n²).
- T ∈ o(n³), but T ∉ o(n²).
- T ∈ ω(n), but T ∉ ω(n²).

## Useful growth rules

- **Polynomials:** if c_d > 0, then c_d·n^d + … + c₁·n + c₀ ∈ Θ(n^d). The highest-degree term wins.
- **Logarithms:** log_b n = log_a n / log_a b, so every base gives Θ(log n). For any a, b > 0, (log n)^a ∈ o(n^b). Every polylog is smaller than every polynomial.
- **Exponentials:** for r > 1 and d > 0, n^d ∈ o(rⁿ). For 1 < r < s, rⁿ ∈ o(sⁿ). Every polynomial is smaller than every exponential.

## The completed table from the lecture

Logs are base 2.

| f(n) | g(n) | Relation |
| :--- | :--- | :--- |
| 2^(n+5) | 2ⁿ + 2⁵ + n^100 | Θ (f = 32·2ⁿ) |
| n⁴ − n³ | 16^(log n) | Θ (16^(log n) = n⁴) |
| 5ⁿ | 10^(2n) | o |
| 5^(n²/2) | 10^(2n) | ω |
| n^(1/log log n) | n^0.001 | o |
| n! | nⁿ | o |
| n log n | log(n!) | Θ |
| n^(log²⁰ n) | 2ⁿ | o |
| ∑ₖ₌₁ⁿ k² | ∑ₖ₌₁ⁿ k | ω (Θ(n³) versus Θ(n²)) |

**Pitfall:** a Θ claim needs both an upper and a lower bound. Showing f ∈ O(g) alone allows f to be much smaller.

These bounds feed into [[concepts/divide-and-conquer/solving-recurrences|recurrence solving]] and the polynomial-time definition in [[concepts/primer/problems-and-efficiency|problems and efficiency]].

Source: `Lecture 1_ Fundamentals of Algorithm Analysis.pdf`, PDF pp. 40–124. The table was checked visually on PDF p. 124.
