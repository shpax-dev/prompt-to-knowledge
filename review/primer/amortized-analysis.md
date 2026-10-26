# Amortized Analysis

A data structure answers a sequence of operations given one at a time, which makes it a *dynamic* problem. Worst-case analysis of a single operation is pessimistic when expensive operations are rare.

**Amortized cost** is the worst-case total cost of a sequence of n operations, divided by n:

```text
amortized cost = (∑ actual cost of operation i) / n
```

## Accounting method

Think of time as money:

- Each operation borrows a fixed number of dollars from the bank. This is its amortized charge.
- Each unit of real work costs $1. Leftover money stays "in your pocket" as credit.
- Expensive operations spend credit that cheap operations saved earlier.

Total actual cost = dollars borrowed − dollars left in the pocket ≤ dollars borrowed. If the pocket never goes negative, the borrowed amount is an upper bound on the true cost.

## Binary counter

Increment a k-bit counter n times, starting from 0. The cost of one increment is the number of bits it flips.

- **Naive worst case:** one increment can flip k bits, so n increments cost O(nk).
- **Amortized:** charge $2 per increment. Setting one bit from 0 to 1 spends $1 and leaves $1 on that bit. Later, resetting that bit from 1 to 0 is paid by its stored dollar.
- **Invariant:** the dollars in the pocket equal the number of 1-bits. So the pocket is never negative, and n increments flip at most 2n bits. That is O(1) amortized per increment.

**Derived check:** 8 increments from 0 flip 1, 2, 1, 3, 1, 2, 1, 4 bits, for 15 total, which is at most 2·8 = 16.

## Lazily initialized array

The goal is O(1) `INIT`, `READ`, and `WRITE`, where unwritten cells read as 0. This assumes `malloc` returns an *uninitialized* block in O(1) time, which is true in C/C++ but not Java.

- `A` stores the values.
- `k` counts how many cells have been written.
- `B[i]` records the order in which cell i was first written.
- `C[j]` records which index was the j-th one written.

`IsValid(i)` returns true exactly when 1 ≤ B[i] ≤ k **and** C[B[i]] = i. The C check is what makes this work. B[i] may hold garbage that happens to land in [1, k], but C confirms the back-pointer. A first write to i increments k and sets B[i] ← k and C[k] ← i.

[[review/primer/union-find|Union–find with path compression]] is the main amortized structure later in the course.

Source: `Lecture 2_ Comprehensive Review of Data Structures.pdf`, PDF pp. 2–54.
