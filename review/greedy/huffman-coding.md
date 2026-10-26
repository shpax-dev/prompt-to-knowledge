# Huffman Coding

## Prefix codes

- **Fixed width:** each symbol in {a, b, c, d, e} gets 3 bits, as in ASCII. Decoding is trivial.
- **Variable width:** shorter codes for frequent symbols save space. But {0, 1, 10, 01, 11} is ambiguous: the string 0010 decodes in several ways.
- **Prefix code:** no codeword is a prefix of another, such as {11, 01, 001, 000, 100}. Decode by scanning left to right and emitting a symbol as soon as a codeword matches. With this code, 1101 decodes as 11 | 01.

A prefix code is the same thing as a binary tree with symbols at the leaves; a symbol's codeword length is its leaf depth. With frequencies fₓ, the **average bits per letter** is:

```text
ABL(T) = ∑ fₓ · depth(x)
```

The goal is to minimize ABL.

## Structure of optimal trees

- **Theorem:** an optimal prefix tree is *full*: every internal node has 2 children. *Exchange:* if node u has one child v, replace u by v. Every leaf below gets one bit shorter, which contradicts optimality.
- **Lemma:** let x and y be the two least frequent symbols. Some optimal tree has x and y as siblings at maximum depth.
  - *Exchange:* a full tree has two sibling leaves a and b at maximum depth d. Swapping x with a changes the cost by Δ·(f(x) − f(a)), where Δ ≥ 0 is the depth difference. Since f(x) ≤ f(a), the cost does not increase. Then do the same for y and b.

## Top-down is not optimal

Shannon–Fano splits the symbols into two sets of nearly equal total frequency and recurses. For frequencies a .32, b .25, c .20, d .18, e .05, it gives ABL 2.25, while the optimum is 2.23.

## Huffman's algorithm (bottom-up)

```text
while more than one symbol remains:
  take the two lowest frequencies x, y
  replace them with a merged symbol w, where f(w) = f(x) + f(y)
then expand each w back into an internal node with children x and y
```

**Optimality:** if T′ is the tree for the reduced alphabet, then ABL(T) = ABL(T′) + f(w). By induction on alphabet size, together with the sibling lemma, Huffman's tree is optimal.

**Runtime:** finding the two minima by scanning gives O(k²) for k symbols. A [[review/primer/binary-heaps|min-heap]] gives O(k log k).

**Derived trace:** for the frequencies above, merge e + d = .23, then c + .23 = .43, then b + a = .57, then .43 + .57. One resulting code is c = 00, d = 010, e = 011, b = 10, a = 11. ABL = 2(.32 + .25 + .20) + 3(.18 + .05) = 2.23.

**Pitfall:** ties can produce different trees and codewords with the same ABL. Compare costs, not codewords.

Huffman was a student in Fano's information-theory class. He solved the open problem as a class project instead of taking the final exam.

Source: `Lecture 8_ Greedy Algorithms Hard Level Problems.pdf`, PDF pp. 37–86.
