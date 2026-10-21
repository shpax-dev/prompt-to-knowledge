---
date-generated: 2026-10-20
date-updated: 2026-10-20
---

# Demand Paging and Working Sets

Demand paging brings a legal page into physical memory when it is needed. A process need not have its entire address space resident before execution. Prefetching/prepaging instead predicts future needs.

## Fault path

1. The access cannot proceed with the current mapping or residency.
2. Hardware transfers control to the OS.
3. The OS determines whether the access is legal, a protection event such as copy-on-write, or an invalid reference.
4. For a legal nonresident page, locate backing data or create a zero-filled page. Obtain a frame, evicting another page if needed.
5. Finish I/O if required, update translation state, and retry the instruction when safe.

A clear present bit alone does not prove a page is stored in swap: it may need zero-fill or have file backing. Likewise, not every page fault requires disk I/O. Keep validity, permissions, and residency separate.

## Locality and allocation

Temporal locality reuses recently accessed data; spatial locality accesses nearby data. A working set W(window,t) is the set of pages referenced in the specified recent execution window.

If active working sets exceed available memory, pages can be evicted shortly before reuse. Frequent faults and page I/O dominate useful execution: **thrashing**. Adding more runnable jobs under memory pressure can make this worse. Reducing concurrency or allocating sufficient frames can help.

Local replacement chooses a victim within a process's allocation. Global replacement can take a frame from another process. Clock is a selection mechanism that can be used in different allocation scopes; it does not by itself require global replacement.

## Copy-on-write

After `fork()`, parent and child may initially share physical pages through mappings marked read-only for copy-on-write. On a write, the OS preserves process isolation by creating a private copy when necessary and adjusting permissions/mappings. Both parent and child must be subject to the copy-on-write mechanism; it is not just a restriction on the child.

Thread stacks and process heaps can have different contents even when some code or kernel mappings are shared. The course's address-space layout is a model, not a universal claim about every platform.

Source: `CS537 Exam 1 Review.pdf`, PDF pp. 38-56. Related: [[review/virtualization/page-replacement|victim policies]], [[review/virtualization/processes-and-limited-direct-execution|fork]], and [[review/virtualization/address-translation-and-page-tables|PTE state]].
