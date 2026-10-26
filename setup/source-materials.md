# Supplied Source Materials

The vault was prepared from 15 lecture PDFs supplied on 2026-10-26. They are not included in this repository, and the original local files were read without modification. Original URLs and redistribution permissions were not provided.

Each title slide reads "CS 577", Department of Computer Sciences, University of Wisconsin–Madison, and names Manolis Vlatakis. The printed term is Spring 2026 on every deck except Lecture 5, which prints Fall 2024.

## Lecture decks

| Original filename | PDF pages | Printed title | Review area |
| :--- | ---: | :--- | :--- |
| Lecture 1_ Fundamentals of Algorithm Analysis.pdf | 132 | Basics of Algorithm Analysis | `review/primer/` |
| Lecture 2_ Comprehensive Review of Data Structures.pdf | 60 | Data Structures & Amortized Analysis | `review/primer/` |
| Lecture 3_ Advanced Data Structures Union-Find and Heaps.pdf | 169 | Union-Find & Binary Heaps | `review/primer/` |
| Lecture 4_ Divide and Conquer Strategies Part A.pdf | 180 | Divide and Conquer: Sort & Search | `review/divide-and-conquer/` |
| Lecture 5_ Divide and Conquer Strategies Part B.pdf | 131 | Divide and Conquer: Applications | `review/divide-and-conquer/` |
| Lecture 6_ Las Vegas Algorithms Randomized Quick Sort and Selection.pdf | 158 | Randomized Algorithms: Quick Algorithms | `review/divide-and-conquer/` |
| Lecture 7_ Greedy Algorithms Easy Level Problems.pdf | 152 | Greedy | `review/greedy/` |
| Lecture 8_ Greedy Algorithms Hard Level Problems.pdf | 134 | More (Hard)/(Interesting) Greedy | `review/greedy/` |
| Lecture 9_ Dynamic Programming A Primer.pdf | 96 | Dynamic Programming Primer | `review/dynamic-programming/` |
| Lecture 10_ Dynamic Programming In Depth Examples and Applications.pdf | 277 | Dynamic Programming | `review/dynamic-programming/` |
| Lecture 11_ Introduction to Graph Algorithms and Traversals.pdf | 155 | Graphs | `review/primer/` |
| Lecture 12_ Shortest Paths and Minimum Spanning Trees.pdf | 138 | Graph Algorithms Part (A) | `review/greedy/`, plus Bellman–Ford in `review/dynamic-programming/` |
| Lecture 13_ All Pair Shortest Paths.pdf | 45 | All Pair Shortest Paths | `review/dynamic-programming/` |
| Lecture 14_ Network Flow and Combinatorial Optimization.pdf | 235 | Network Flow | `review/network-flow/` |
| Lecture 15_ NP Reductions and Intractability.pdf | 216 | Computational Intractability | `review/np-completeness/` |

The lectures are grouped by technique, not by lecture number:

- Graph traversal (Lecture 11) sits in the primer because later topics depend on it.
- Dijkstra and minimum spanning trees are greedy algorithms.
- Bellman–Ford and all-pairs shortest paths are dynamic programs.
- Network flow has its own area, because its reductions lead into NP-completeness.

No past exams, solution keys, or homework handouts were supplied, so `exam-prep/` contains only study guides and original practice.

## Reading the provenance

- Each concept note lists its supporting PDF and its relevant 1-based PDF page ranges. These are study-topic references, not claims that every line is quoted from those pages.
- The slides build up incrementally, so a single idea often spans a run of near-duplicate pages. Page ranges cover the whole run.
- The text was machine-extracted. Slide titles use a decorative font that does not extract cleanly, and several pages are mostly images:
  - Lecture 3, PDF pp. 39–44;
  - Lecture 11, the Kosaraju walkthrough on PDF pp. 73–115;
  - Lecture 13, PDF pp. 6–22;
  - gadget figures in Lecture 15.
- Pages whose wording or numbers matter were checked visually and are marked "checked visually" in the notes.
- Lecture 14, PDF pp. 126–141, restates circulations and lower bounds in a different slide style from the rest of the deck.

## Unresolved source items

These are recorded but deliberately not "corrected":

- **Lecture 13, PDF p. 40:** the Johnson reweighting is printed as ŵ(u, v) = w(v, u) + h(v) − h(u). That differs from the usual form, w(u, v) + h(u) − h(v). See [[review/dynamic-programming/all-pairs-shortest-paths|all-pairs shortest paths]].
- **Lecture 7, PDF pp. 81–83:** the greedy coin trace for $2.51 skips the 50¢ coin, although it was listed as a denomination two slides earlier. That example is not reused in the notes.

If you keep the PDFs locally, copy them unchanged into a `materials/` folder beside the review area they support, and keep the original filenames. Check redistribution permission before publishing course files or substantial copied course content.
