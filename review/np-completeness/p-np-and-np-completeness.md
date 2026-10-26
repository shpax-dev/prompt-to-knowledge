# P, NP, and NP-Completeness

## P

Encode an instance as a binary string s with |s| bits. An algorithm runs in polynomial time if it takes O(poly(|s|)) steps in the worst case. **P** is the set of decision problems that have a polynomial-time algorithm.

## Efficient certification and NP

A **certifier** B(s, t) for problem X takes an instance s and a proposed proof t that s is a yes-instance. It is efficient if:

- B runs in polynomial time, and
- s is a yes-instance exactly when some t with |t| ≤ poly(|s|) makes B(s, t) answer yes.

B(s, t) = no means only that *this particular t* is not a valid proof; it does not mean s is a no-instance. Trying every t gives a brute-force algorithm.

**NP** is the set of problems with an efficient certifier. The name means *nondeterministic polynomial time*: a machine that could try every certificate at once would run in polynomial time.

**P ⊆ NP:** if A solves X in polynomial time, let B(s, t) ignore t and return A(s).

Whether P = NP is one of the seven Clay Mathematics Institute Millennium Prize Problems.

## NP-hard and NP-complete

- **NP-hard:** every Y ∈ NP satisfies Y ≤ₚ X. X itself need not be in NP.
- **NP-complete:** X is NP-hard *and* X ∈ NP.

**Theorem:** if X is NP-complete, then X is solvable in polynomial time if and only if P = NP.

- If P = NP, then X ∈ NP = P.
- If X ∈ P, every NP problem reduces to X, so every NP problem is in P.

## The first NP-complete problem

**Cook–Levin (1971/1973, paraphrased): Circuit Satisfiability (CSAT) is NP-complete.**

A circuit is a DAG of AND, OR, and NOT gates with some inputs fixed and some free. CSAT asks whether some setting of the free inputs makes the output 1.

- **CSAT ∈ NP:** the input size is Θ(|V|). Given values for the inputs, evaluate the gates in topological order in O(|V|).
- **Every X ∈ NP reduces to CSAT:** turn X's certifier B_X into a circuit. Hard-wire the n input bits of s, leave the poly(n) bits of t free, and let the gates implement B_X. The output is 1 exactly when some certificate is accepted.

## How to prove that a new problem X is NP-complete

1. Prove that X ∈ NP: give a certificate and a polynomial-time check.
2. Choose a known NP-complete problem Y.
3. Prove Y ≤ₚ X with a **Karp reduction**:
   - build an instance s_X from any instance s_Y in polynomial time;
   - prove that if s_Y is yes, then s_X is yes;
   - prove that if s_X is yes, then s_Y was yes.

## 3SAT is NP-complete

- **In NP:** the certificate is a truth assignment.
- **CSAT ≤ₚ 3SAT:** give every circuit node v a variable xᵥ, and add clauses that force each gate's value.
  - NOT with input u: (xᵥ ∨ xᵤ) ∧ (¬xᵥ ∨ ¬xᵤ).
  - OR with inputs u, w: (xᵥ ∨ ¬xᵤ) ∧ (xᵥ ∨ ¬x_w) ∧ (¬xᵥ ∨ xᵤ ∨ x_w).
  - AND with inputs u, w: (¬xᵥ ∨ xᵤ) ∧ (¬xᵥ ∨ x_w) ∧ (xᵥ ∨ ¬xᵤ ∨ ¬x_w).
  - Add a unit clause for each constant input, and (x_output) for the output.
  - Pad short clauses to length 3 with variables z₁, z₂ that are forced to 0. Doing that takes helpers z₃, z₄ and 8 more clauses.
- **Correctness:** a satisfying circuit input determines the value of every gate, which satisfies all the clauses. Conversely, a satisfying assignment gives circuit inputs whose gate values match the clauses.

The gate clauses were checked visually against PDF p. 115, because text extraction drops the negation bars.

**Corollary:** 3SAT, IS, VC, SC, and SP are NP-complete, via the chains in [[review/np-completeness/polynomial-time-reductions|polynomial-time reductions]].

## Beyond NP

- **coNP:** X ∈ coNP when its complement is in NP. Certification is asymmetric: one certificate proves "yes", but proving "no" may need something else. If X ∈ P, its complement is in P too.
  - **Open:** does NP = coNP? Does P = NP ∩ coNP?
  - **Theorem:** if NP ≠ coNP, then P ≠ NP. Proof via the contrapositive: if P = NP, then NP is closed under complement.
- **PSPACE:** problems solvable with polynomial memory. P ⊆ PSPACE and NP ⊆ PSPACE: for 3SAT, try every assignment while reusing one n-bit counter.
  - Quantified SAT, ∃x₁ ∀x₂ ∃x₃ … φ, is PSPACE-complete. It models contingency planning.

Source: `Lecture 15_ NP Reductions and Intractability.pdf`, PDF pp. 59–117 and 194–213.
