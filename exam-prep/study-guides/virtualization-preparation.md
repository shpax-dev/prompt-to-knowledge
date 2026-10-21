# Virtualization Preparation

This guide follows the topics in `CS537 Exam 1 Review.pdf`. It is a skill checklist, not a statement of the next exam's scope or rules.

## Prepare by mechanism

1. Explain [[review/virtualization/processes-and-limited-direct-execution|processes and limited direct execution]]: user/kernel mode, timer interrupts, ready versus blocked, `fork()`, `exec()`, and `wait()`.
2. Trace [[review/virtualization/cpu-scheduling|CPU schedules]] using arrival times, remaining bursts, first-run times, and completion times. Practice FIFO, SJF, STCF, RR, and MLFQ accounting.
3. Perform [[review/virtualization/address-translation-and-page-tables|address translation]] with bounds, segment identifiers, page offsets, PTE permissions, and multilevel indices.
4. Calculate [[review/virtualization/tlb-and-locality|TLB misses]] from actual page accesses and initial cache contents.
5. Trace [[review/virtualization/page-replacement|replacement]] and distinguish it from [[review/virtualization/demand-paging-and-working-sets|demand paging, copy-on-write, and thrashing]].

## A productive study cycle

Read one concept, close the note, solve a small trace, then compare the intermediate state with the explanation. Explain why a plausible alternative answer is wrong. Return to the specific mechanism behind a mistake instead of rereading the entire collection.

Useful source locations: scheduling practice pp. 63-65, 79-81, and 104-105; translation and table sizing pp. 71-74 and 106-113; replacement pp. 87 and 114-115. These are PDF page numbers, not universal question identifiers.

## Self-check

Can you explain why a timer interrupt need not switch processes, why successful `exec()` keeps a PID, why a TLB miss need not be a page fault, and why a multilevel walk needs an additional data access after reading its PTEs?

Work [[exam-prep/practice/operating-systems-practice|original practice questions]] 1-4 without opening the solutions. No score or mastery status is assumed.

Source: `CS537 Exam 1 Review.pdf`, pp. 1-115; its embedded exams have their own instructions and some duplicate/annotated pages. Check the containing PDF page before using a solution or timing rule.
