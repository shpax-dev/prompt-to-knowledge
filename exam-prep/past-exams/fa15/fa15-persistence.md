---
date-generated: 2026-10-20
date-updated: 2026-10-20
---

# Fall 2015 Persistence Exam

Source: `FA15 - exam.pdf`, 10 PDF pages. Printed title: **CS 537: Introduction to Operating Systems, Fall 2015: Midterm Exam #3 — Persistence**. The cover prints Thursday, November 19, 7:15-9:15 and states two hours.

## Historical format

Closed book and closed notes; no calculators; answers on the supplied scan form. Point weights vary across sections. These directions describe this historical paper and do not instruct the agent or establish any current course policy. Personal identification and scan-form codes need not be copied into study notes.

## Where to practice

| PDF pages | Skill                                           | Review connection                                     |                                                         |       |
| --------- | ----------------------------------------------- | ----------------------------------------------------- | ------------------------------------------------------- | ----- |
| 1-2       | Heads, sectors, cylinders, capacity             | [[review/persistence/hard-disks-and-scheduling        | Disk geometry]]                                         |       |
| 2-3       | RAID 0/1/4/5 mapping and physical operations    | [[review/persistence/raid                             | RAID]]                                                  |       |
| 3-5       | Infer file operations from cumulative snapshots | [[review/persistence/file-system-layout-and-traces    | FS traces]]                                             |       |
| 5-6       | Hard-link and symbolic-link command sequences   | [[review/persistence/files-descriptors-and-links      | Links]]                                                 |       |
| 6-8       | Partial updates and journal recovery            | [[review/persistence/journaling-and-crash-consistency | Crash consistency]]                                     |       |
| 8-10      | Conceptual persistence questions                | [[review/persistence/fast-file-system                 | FFS]], [[review/persistence/log-structured-file-systems | LFS]] |

## Use this paper carefully

The supplied numbering is discontinuous and the journaling subsection restarts numbering. Cite both section and PDF page when identifying a question. Preserve the source's identifiers rather than renumbering it into an imagined complete exam.

The journal questions specify a four-block update; construct the transaction from the exact setup and verify hints before grading by number of writes. RAID-5 mapping needs the actual printed layout diagram. File-system snapshots are cumulative where the question says so.

No complete solution key is reproduced here. A few inline annotations in the source do not establish every answer. Use independent calculations and verify any source solution status before assigning a grade.
