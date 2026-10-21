---
date-generated: 2026-10-20
date-updated: 2026-10-20
---

# Page Replacement

When a needed page must be brought into a full set of frames, a replacement policy selects a victim. Keep this problem separate from replacement of translations in a [[review/virtualization/tlb-and-locality|TLB]].

| Policy | Victim | What to track |
| --- | --- | --- |
| OPT / MIN | Page whose next use is farthest in the future, or never | Remaining reference stream; an offline benchmark |
| FIFO | Page resident longest | Insertion order; hits do not refresh it |
| LRU | Page used least recently | Recency order; hits refresh it |
| Clock | A page with reference bit clear when scanned | Circular hand and reference bits |

## Trace method

Begin from the specified frame contents, not an assumed empty set. For each reference, record hit/miss, victim if any, and the new state. Separate reference order from FIFO insertion order. Include initial compulsory misses unless the question excludes them.

**Original practice:** three initially empty frames and stream A, B, C, A, D, A.

- FIFO misses on A, B, C, D, A: 5 misses. The hit on A does not move it out of the oldest position before D arrives.
- LRU misses on A, B, C, D: 4 misses. The hit on A makes B the least recent page before D arrives.

## Clock and dirty pages

An access sets a reference bit. On replacement, the hand clears a set reference bit and continues; it chooses an eligible clear-bit frame. The result depends on initial hand position and bit values. A dirty bit says the page was modified, not whether it was recently accessed. Clean, backed pages may be discarded without a write; dirty victims may require writeback.

OPT and LRU have the stack property: with consistent policy conventions, more frames do not increase their misses on the same stream. FIFO can exhibit Belady's anomaly. This does not imply LRU outperforms every other policy on every workload.

Source: `CS537 Exam 1 Review.pdf`, PDF pp. 38-50 and 56; embedded exercises, pp. 87 and 114-115. Related: [[review/virtualization/demand-paging-and-working-sets|working sets and thrashing]].
