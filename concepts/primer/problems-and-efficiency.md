## Problems and Efficiency

A computational problem is a relation between valid input instances and acceptable outputs. Interesting problems have infinitely many instances; otherwise a lookup table would solve them.

### Decision and Optimization

- **Decision problems** answer yes or no. Reachability asks: is there an s–t path in G?
- **Optimization problems** ask for the best solution. Shortest path asks: what is the shortest s–t path, given edge lengths?

The output can be a yes/no answer, a value such as a path length, or a description of the solution itself. Say which one you mean before analyzing an algorithm. [[concepts/np-completeness/polynomial-time-reductions|Reductions]] move between the two forms.

### What Makes an Algorithm Acceptable

1. **Soundness:** every answer it returns is correct.
2. **Completeness:** it returns an answer for every valid instance.
3. **Resource use:** time, space, energy, or other costs.

### Defining "Efficient"

The lecture tries three definitions in turn:

| Attempt | Problem with it |
| :--- | :--- |
| Runs quickly on real inputs | "Quickly" and "real inputs" are not concrete. |
| Qualitatively better worst case than brute force | "Qualitatively" is still vague. |
| **Polynomial running time in the input size** | The working definition. Doubling the input slows the algorithm by at most a constant factor. |

The polynomial definition is not perfect. n^100 is polynomial, while n^(1 + 0.02 log n) is not, even though the second grows more slowly for practical n.

**Brute force** enumerates every candidate solution and keeps the best. Stable matching is the motivating example: with n hospitals and n doctors there are n! perfect matchings. The input is two n × n preference arrays, so it already has size Θ(n²). Gale–Shapley makes at most n² proposals because each hospital proposes to each doctor at most once. Linear time in the input size therefore means O(n²) here, not O(n).

### Model of Computation

Running times count primitive operations in the **word RAM** model:

- Each memory cell holds a w-bit integer with w ≥ log₂ n, so an index fits in one word.
- Arithmetic, comparisons, array indexing, pointer following, and branches each cost O(1).
- Memory is the number of cells used.

Some problems need a finer model. Multiplying n-bit integers counts bit operations, as in [[concepts/divide-and-conquer/fast-multiplication|fast multiplication]].

### Worst, Average, and Best Case

For an algorithm A on inputs of size n:

- **Worst case:** the maximum T_A(i) over inputs with |i| = n. This is an absolute guarantee.
- **Average case:** the expected T_A(i) under a distribution over inputs. If no distribution is named, assume uniform. It is usually harder to analyze.
- **Best case:** the minimum. It is rarely meaningful on its own, but can break ties between otherwise equal algorithms.

The worst-case complexity of a *problem* is the minimum, over all algorithms that solve it, of their worst-case running time. An algorithm gives an upper bound for the problem. A [[concepts/divide-and-conquer/lower-bounds|lower bound]] must hold for every possible algorithm.

Continue with [[concepts/primer/asymptotic-notation|asymptotic notation]] to compare growth rates.

Source: `Lecture 1_ Fundamentals of Algorithm Analysis.pdf`, PDF pp. 2–39 and 125.
