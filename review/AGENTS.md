# Review Agent Instructions

Read the root `AGENTS.md` first. This file governs explanations and concept documentation under `review/`.

## Teaching and retrieval

Find and read the canonical concept note, and its cited source, before making a source-specific claim. Start with the mechanism, then explain why it works and what it costs. Use a small trace or table when it shows the behavior better than prose. Link related concepts in context; the review areas are cross-linked on purpose, for example Dijkstra in `greedy/`, Bellman–Ford in `dynamic-programming/`, and union–find in `primer/`.

For questions, answer in chat without writing notes. For an explicit documentation request, update the focused concept note. Append only genuinely new information, and merge repeated agent-generated explanation into the existing section where appropriate. Preserve user-authored observations.

## Technical conventions

- **Model:** state the computational model (word RAM or comparison model), what n and m measure, and whether a bound is worst-case, average-case over inputs, expected over the algorithm's coins, or amortized.
- **Notation:** use O, Ω, Θ, o, and ω precisely. Claim Θ only after proving both directions. Distinguish an algorithm's bound from a problem's bound, and polynomial from pseudo-polynomial time, such as O(nW) or O(mC).
- **Divide and conquer:** give the number of recursive calls, their size, the combine cost, the recurrence, and how it was solved.
- **Greedy:** name the rule, give counterexamples for rejected rules, and name the proof technique (stays ahead or exchange). Say what "optimal" means when several optimal solutions exist.
- **Dynamic programming:** give definitions, the meaning of one table cell, the Bellman equation with base cases, the fill order, the location of the answer, runtime as cells × work per cell, and reconstruction.
- **Graphs:** state directed or undirected, the sign of the weights, and whether negative cycles are possible. Keep source-based and sink-based shortest-path formulations distinct.
- **Flow and reductions:** state the reduction direction explicitly (Y ≤ₚ X means a solver for X solves Y), and prove both directions of correctness plus polynomial construction time.
- **Pseudocode:** treat it as teaching pseudocode. State 0-based or 1-based indexing when it matters, as in heap arrays.

## Working with the supplied PDFs

The lecture slides build up incrementally, use a decorative title font that does not extract cleanly, and contain image-only diagrams. Text extraction is an aid, not an authority, for formulas, negation bars, subscripts, table values, or answers marked by color or checkmarks. Inspect the rendered page when a value matters. Repeated build-up pages are not independent evidence. Use 1-based PDF page numbers and preserve the printed term.

A useful concept note usually contains a concise explanation, the key recurrence or invariant, a small example when helpful, a specific pitfall, and source provenance. Include only sections that add information. Do not create empty templates or copy full slide sequences.

## Validation

Check arithmetic and traces independently, ideally by recomputation. Trace each table entry and each data-structure state. If an apparent source mistake changes an explanation or answer, ask about it before editing the affected claim, and list it under the unresolved items in `setup/source-materials.md`. Do not silently "correct" source content or erase a student's reasoning. Report unreadable material and missing specifications explicitly.
