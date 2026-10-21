# Fall 2023 Persistence Exam

Source: `FA23 - exam.pdf`, 11 PDF pages. Printed title: **CS 537: Introduction to Operating Systems, Fall 2023: Midterm Exam #3**.

## Historical format

The cover states 60 equally weighted questions and 75 minutes, closed book and closed notes, with no calculators. It assumes a single CPU, byte addressing, binary KB/MB, and optimally aligned data unless a question says otherwise. These are that paper's rules only.

## Question map

| PDF pages | Questions | Skills |
| --- | --- | --- |
| 2-3 | 1-30 | Persistence true/false: devices, disks, RAID, metadata, recovery, flash, integrity |
| 3-4 | 31-34 | [[review/persistence/hard-disks-and-scheduling|Disk geometry and capacity]] |
| 4 | 35-38 | Disk scheduling and head movement |
| 5-7 | 39-43 | [[review/persistence/file-system-layout-and-traces|Cumulative FS snapshots]] |
| 7-8 | 44-49 | [[review/persistence/journaling-and-crash-consistency|Partial-update failures]] |
| 8-9 | 50-54 | Metadata-journal recovery |
| 9-10 | 55-57 | [[review/persistence/checksums-and-data-integrity|Checksums and identity]] |
| 10 | 58-59 | [[review/persistence/log-structured-file-systems|LFS operation and liveness]] |
| 11 | 60 | [[review/persistence/ssds-and-flash-translation|SSD sustained-write behavior]] |

## Preparation value

Explain every true/false answer with the condition that makes it true or false; do not ingest the listed statements as facts. For SCAN/C-SCAN, inspect whether the problem defines disk endpoints or a request-boundary convention before counting movement.

File-system snapshots need both directory-entry changes and inode state. For LFS, follow the final checkpoint to the current map and inode versions. A programmed or previously valid block is not necessarily live now.

The SSD problem includes a throughput graph and an SLC cache size; inspect the figure before diagnosing it. This note does not reproduce the graph or provide a complete answer key. Practicing under 75 minutes simulates this particular paper, not a confirmed future exam.
