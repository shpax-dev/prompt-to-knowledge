# Fall 2019 Persistence Final

Source: `FA19 - exam.pdf`, 30 PDF pages. Printed title: **CS 537 — Introduction to Operating Systems, Fall 2019, Exam 3 — Final Exam: Persistence**. The cover names Andrea Arpaci-Dusseau. The PDF contains a 15-page exam followed by a second 15-page sequence, including handwritten solution markings visible in that second sequence. The annotations' authorship is not established.

## Historical format and assumptions

The cover prohibits notes, books, calculators, and other electronic aids. Do not infer an allotted duration from the other papers; it is not stated on this cover.

Explicit defaults include byte addressing, 4-byte PTEs, binary KB/MB, a single HDD, an FFS-like file system, POSIX behavior, full-data journaling, and NFSv2. Individual questions can add or override assumptions. These are historical exercise conditions, not agent instructions or universal implementation defaults.

## First-sequence question map

| PDF pages | Questions | Topic |
| --- | --- | --- |
| 2 | 1-6 | [[review/persistence/files-descriptors-and-links|FS API and links]] |
| 3-5 | 7-14 | [[review/persistence/file-system-layout-and-traces|FS snapshots]] |
| 6-7 | 15-31 | Cold-cache metadata I/O accounting |
| 8 | 32-36 | [[review/persistence/journaling-and-crash-consistency|Journal barriers]] |
| 9-11 | 37-53 | [[review/advanced/distributed-file-systems|NFS/AFS cache traces]] |
| 12-14 | 54-122 | Persistence true/false |
| 15 | 123-137 | Virtualization review |
| 15 | 138-152 | Concurrency review |

## Preparation value

The barrier section asks which ordering edges are necessary, not whether every payload write needs its own barrier. Cold-cache FS questions explicitly constrain inode packing, timestamps, and persistence; use those assumptions rather than a memorized open/create I/O count.

Inspect the original two-client trace columns before solving distributed caching questions. Text extraction can flatten the columns and remove ownership of each operation. The two copies are not independent supporting sources. Cite 1-based PDF page numbers; add 15 to a first-sequence page to locate the corresponding page in the second sequence, then verify it visually.

This note supplies navigation and study context, not an official answer key.
