# Binary Heaps

A binary min-heap is a priority queue. It is a **complete** binary tree, meaning every level is full except possibly the last, which fills from left to right. The root of every subtree holds that subtree's minimum.

## Why a heap

The motivating problem is earliest-deadline-first scheduling, which needs FindMin, DeleteMin, Insert, Update, Delete, and Build:

| Structure | FindMin | Insert |
| :--- | :--- | :--- |
| Unsorted array | Θ(n) | O(1) |
| Sorted array | Θ(1) | Θ(n) |
| Binary heap | Θ(1) | O(log n) |

## Array layout

A complete tree needs no pointers. With 0-based indices:

```text
children of i: 2i + 1 and 2i + 2
parent of i:   ⌊(i − 1) / 2⌋
```

A complete tree of height h has up to 2^(h+1) − 1 nodes, so its height is Θ(log n). About half of the nodes are leaves.

## Operations

- **Insert:** append at the end, then *shift up* (swap with the parent) while the new key is smaller. O(log n).
- **DeleteMin:** move the last element to the root, shrink the array, then *shift down* (swap with the smaller child) while needed. O(log n).
- **Update:** if a key decreases, shift up. If it increases, shift down. Only the affected branch can break.
- **Delete(position):** swap with the last element, remove the last element, then restore the heap order at that position. The lecture restores with a shift-down. *Derived note:* when the deleted position is not the root, the moved element can also be smaller than its new parent, and then it must shift up instead.

## Building a heap

- **One insert at a time:** n × O(log n) = O(n log n).
- **Heapify:** treat the array as a tree and shift down from the last non-leaf back to the root. A node at height h costs O(h), and most nodes are near the bottom:

```text
(n/2)·0 + (n/4)·1 + (n/8)·2 + … + 1·log n = O(n)
```

## Applications

- **Heapsort:** heapify in O(n), then extract the root n times in O(log n) each, for O(n log n) total. Repeatedly extracting from a min-heap gives ascending order. In-place heapsort usually uses a max-heap and swaps each extracted root to the end.
- **k-th smallest:** the lecture gives two bounds, Θ(n) + O(k log n) offline and Θ(k) + O(n log k) online. *Derived descriptions:* the offline version heapifies all n elements and extracts k times. The online version keeps a size-k max-heap of the smallest elements seen so far and replaces its root when a smaller element arrives.

## Original practice trace

Insert 7, 4, 9, 1, 6, 3 into an empty min-heap:

```text
after 7: [7]
after 4: [4, 7]
after 9: [4, 7, 9]
after 1: [1, 4, 9, 7]        (1 shifts up twice)
after 6: [1, 4, 9, 7, 6]
after 3: [1, 4, 3, 7, 6, 9]  (3 swaps with 9)
DeleteMin → 1, then [3, 4, 9, 7, 6]
```

Heaps power [[review/greedy/dijkstra-shortest-paths|Dijkstra's algorithm]], [[review/greedy/minimum-spanning-trees|Prim's algorithm]], and [[review/greedy/huffman-coding|Huffman coding]].

Source: `Lecture 3_ Advanced Data Structures Union-Find and Heaps.pdf`, PDF pp. 45–166.
