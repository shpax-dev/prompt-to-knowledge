# Randomized Algorithms and Probability Tools

## Average case versus randomized

- **Average-case analysis:** the *input* is random, drawn from some distribution D, and you bound the expected cost under D. The assumption may be unrealistic. Uniformly random input orderings rarely happen in practice.
- **Randomized algorithm:** the *algorithm* flips coins. The guarantee then holds for every input, in expectation over the algorithm's own choices.

## Kinds of randomized algorithm

| Type | Correctness | Running time |
| :--- | :--- | :--- |
| Monte Carlo | Correct with probability p; repeat to raise the probability, or give an approximation guarantee in expectation | Bounded |
| Las Vegas | Always correct, or reports failure | Random, polynomial in expectation |
| Atlantic City | Probabilistic | Probabilistic |

Randomized quicksort is Las Vegas: the output is always sorted, and only the running time varies.

## Probability review

- **Sample space** Ω with masses p(i) ≥ 0 summing to 1. An **event** is a set of outcomes, with Pr[E] = ∑ over i ∈ E of p(i), and Pr[not E] = 1 − Pr[E]. For a fair 4-sided die, Pr[roll 1] = 1/4 and Pr[roll 2, 3, or 4] = 3/4.
- **Conditional probability:** Pr[E | F] = Pr[E ∩ F] / Pr[F].
- **Independence:** Pr[E ∩ F] = Pr[E]·Pr[F]. For independent events, the probability that all of them happen is the product.
- **Union bound:** Pr[E₁ ∪ … ∪ Eₙ] ≤ ∑ Pr[Eᵢ], with equality only when the events are mutually exclusive.
- **Random variable:** a function from outcomes to numbers. An **indicator** variable is 1 if an event happens and 0 otherwise, and E[indicator] = Pr[event].
- **Expectation:** E[X] = ∑ j·Pr[X = j]. For a fair 4-sided die, E[X] = 2.5.
- **Linearity of expectation:** E[X + Y] = E[X] + E[Y] and E[aX] = aE[X], *even when X and Y are dependent*. For independent X and Y, E[XY] = E[X]·E[Y].
- **Variance:** Var(X) = E[(X − μ)²]. It measures spread around the mean μ.

## Concentration inequalities

Two distributions can share a mean while one is far more certain. Concentration bounds limit how far a variable strays from its mean.

- **Markov** (X ≥ 0): Pr[X ≥ a] ≤ E[X]/a.
- **Chebyshev** (finite variance σ²): Pr[|X − μ| ≥ kσ] ≤ 1/k².
- **Chernoff** (X a sum of independent Bernoulli variables, μ = E[X], 0 < δ < 1): Pr[X ≥ (1 + δ)μ] ≤ exp(−δ²μ/3), and Pr[|X − μ| ≥ δμ] ≤ 2·exp(−δ²μ/3).

Each step down the list needs more information about X and gives a much stronger bound.

**Pitfall:** Chernoff bounds need independence. The comparison indicators in quicksort are dependent, which is why the lecture leaves the high-probability bound to homework.

The main application is [[concepts/divide-and-conquer/quicksort-and-its-analysis|randomized quicksort]].

Source: `Lecture 6_ Las Vegas Algorithms Randomized Quick Sort and Selection.pdf`, PDF pp. 83–117 and 134–144.
