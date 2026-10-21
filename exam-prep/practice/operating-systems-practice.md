# Original Operating Systems Practice

These are **Original practice** questions created for this starter, inspired by the mechanisms in the supplied material. They do not reproduce the historical exam questions and are not an official exam. State assumptions and show intermediate work.

The separate [[exam-prep/practice/operating-systems-solutions|derived solutions]] contain answers. In quiz mode, present one question at a time without opening or quoting the answer in the user's prompt.

## 1. Scheduling

One CPU, no I/O or switching overhead. A arrives at time 0 with burst 6; B arrives at time 1 with burst 2; C arrives at time 2 with burst 1. Under STCF, keep the running job when remaining times tie. Give execution intervals, mean turnaround, and mean response.

## 2. Address translation

Use 16-bit virtual addresses, 256-byte pages, and a valid readable mapping VPN `0x12` → PFN `0x34`. Translate VA `0x12AB`. Separately, with 4-byte PTEs, calculate the size of a complete linear page table for this address space.

## 3. TLB locality

An initially empty, fully associative TLB has two entries and LRU replacement. Access the page sequence A, B, A, C, B, A. Give hit/miss outcomes and total misses. Assume all pages are valid and resident.

## 4. Page replacement

Three frames start empty. Reference A, B, C, A, D, A. Count page misses for FIFO and LRU. Do not refresh FIFO insertion order on a hit.

## 5. Condition variables

A consumer locks the mutex, checks `count==0` with `if`, waits on a CV, and then removes an item. Why can that be wrong with two consumers even if the producer signals after adding an item? State what `cond_wait()` does to the mutex.

## 6. Semaphore initialization

An initially empty bounded buffer has five slots and multiple producers and consumers. Give initial values for empty-slot, full-slot, and mutex semaphores. Show the ordering of wait/post operations for one producer, including the protected buffer update.

## 7. Deadlock

T1 acquires A then B; T2 acquires B then C; T3 acquires C then A. All locks are single-instance, blocking mutexes and are held until the thread obtains both and finishes. Show a deadlocking schedule and give one global lock order that prevents it.

## 8. Disk latency

A disk rotates at 12,000 RPM, has 3 ms average seek, and transfers at 100 MiB/s. Estimate a random 8 KiB read using average rotational delay, ignoring controller and queueing overhead. Give the transfer time and total in ms.

## 9. RAID mapping

A four-disk RAID 0 uses one-block chunks and zero-based disk and logical block numbering. Where is logical block 19 stored? Separately, in a five-disk RAID 5 with equal disk capacity 2 TiB, what is usable capacity and how many arbitrary disk failures can it tolerate?

## 10. Names and inodes

Initially only path `a` names an openable regular file containing X. Create hard link `b` to `a`; rename `a` to `c`; append Y through `b`; unlink `b`. Which names remain and what contents does `c` have? Assume all calls succeed and no other links or open references remain after the operations.

## 11. Cold pathname lookup

To open `/x/y/z`, assume no cached blocks; root and each directory occupy one data block; each inode occupies a separate block; all components exist; timestamps are ignored. Count block reads required to load z's inode. Do not read z's contents.

## 12. Journal ordering

A full-data redo transaction consists of a header, three payload blocks, and a commit block. Each write completes durably before the next; no home writes begin before commit. What is recoverable after four writes versus five writes? Why must the log not be reclaimed before home updates are durable?

## 13. LFS liveness

A valid checkpoint selects imap block 40, which maps inode 2 to block 35. The current inode at block 35 has data pointers [10, 33]. An older inode at block 20 points to [10, 11], and an obsolete imap block 25 maps inode 2 to block 20. No other current object references blocks 11, 20, or 25. Among blocks 10, 11, 20, 25, 33, 35, and 40, which are live?

## 14. Flash update

The FTL maps logical page 7 to programmed physical page 3 containing A. Physical page 9 is erased and available. A log-based update writes B to logical page 7 using page 9. What changes in the mapping, read result, and liveness of page 3? Must the block containing page 3 be erased immediately?

## 15. Checksums

For bytes `0x12`, `0x34`, `0x56`, and `0x78`, calculate a one-byte XOR checksum and a one-byte additive checksum modulo 256. Can a matching checksum by itself prove that this is the newest committed version of a block?

## 16. Virtualization and networks

Explain guest virtual → guest physical → host physical translation. Contrast a VM with a container's kernel relationship. Finally, if an RPC request executes but its reply is lost, does retrying necessarily preserve exactly-once application behavior? Explain using a non-idempotent operation.
