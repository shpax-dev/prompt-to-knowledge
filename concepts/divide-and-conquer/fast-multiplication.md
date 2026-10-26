# Fast Exponentiation and Multiplication

These three problems share one recipe. Ask the standard divide-and-conquer questions: how many recursive calls, on what size, and what does each level cost? Then cut the number of recursive calls using algebra.

## Fast exponentiation

Compute xⁿ, counting each multiplication as one step. The naive product x·x·…·x uses n − 1 multiplications, which is O(n).

```text
x⁰ = 1
xⁿ = (x^(n/2))²   if n is even
xⁿ = x · x^(n−1)  if n is odd
```

Each case makes one recursive call, and an odd step is always followed by an even one. So T(n) ≤ T(n/2) + O(1) = O(log n).

In the real world, multiplying large numbers is not O(1). That is why integer multiplication below counts bit operations.

## Integer multiplication (Karatsuba)

Multiply two n-bit numbers, counting bit operations. Grade-school partial products cost O(n²).

Split each number into its high and low halves: x = x₁·2^(n/2) + x₀ and y = y₁·2^(n/2) + y₀. Then

```text
xy = x₁y₁·2ⁿ + (x₁y₀ + x₀y₁)·2^(n/2) + x₀y₀
```

- **Four products** of size n/2, plus O(n) for shifts and additions: T(n) ≤ 4T(n/2) + cn = O(n²). No gain.
- **Three products:** compute p = (x₁ + x₀)(y₁ + y₀). Then x₁y₀ + x₀y₁ = p − x₁y₁ − x₀y₀, so T(n) ≤ 3T(n/2) + O(n) = O(n^(lg 3)) ≈ O(n^1.59).

**Derived decimal check:** 1234 × 5678 with x₁ = 12, x₀ = 34, y₁ = 56, y₀ = 78.

```text
x₁y₁ = 672      x₀y₀ = 2652      p = 46 × 134 = 6164
middle = 6164 − 672 − 2652 = 2840
672·10⁴ + 2840·10² + 2652 = 7,006,652 ✓
```

## Matrix multiplication (Strassen)

The naive triple loop for n × n matrices costs O(n³). Splitting into 2 × 2 blocks [a b; c d]·[e f; g h] needs 8 block products plus O(n²) additions: T(n) ≤ 8T(n/2) + cn² = O(n³). No gain.

Strassen (1969) uses 7 products:

```text
p1 = a(f − h)      p2 = (a + b)h      p3 = (c + d)e      p4 = d(g − e)
p5 = (a + d)(e + h)      p6 = (b − d)(g + h)      p7 = (a − c)(e + f)

top-left  = p5 + p4 − p2 + p6      top-right    = p1 + p2
bot-left  = p3 + p4                bottom-right = p1 + p5 − p3 − p7
```

This gives T(n) ≤ 7T(n/2) + cn² = O(n^(lg 7)) ≈ O(n^2.81). The lecture's 2 × 2 example, [1 2; 3 4]·[4 3; 2 1] = [8 5; 20 13], is reproduced exactly by these formulas (derived check).

The slide lists O(n^2.373) as the current best exponent, credited to Virginia Vassilevska Williams. That is a research claim that changes over time, not something proved in this course.

**Pitfall:** saving one recursive call only matters because the recursion repeats at every level; the extra additions add only a lower-order term. Check the recurrence with the [[concepts/divide-and-conquer/solving-recurrences|Master theorem]].

Source: `Lecture 5_ Divide and Conquer Strategies Part B.pdf`, PDF pp. 5–59. Reduction framing: `Lecture 4_ Divide and Conquer Strategies Part A.pdf`, PDF pp. 2–4.
