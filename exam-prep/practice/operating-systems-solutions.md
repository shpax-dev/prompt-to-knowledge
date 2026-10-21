---
date-generated: 2026-10-20
date-updated: 2026-10-20
---

# Derived Solutions to Original Practice

These are **Derived solutions** to [[exam-prep/practice/operating-systems-practice|the original practice set]], not a source or official exam answer key. Each follows the assumptions stated in its question.

## 1. Scheduling

Intervals: A [0,1), B [1,3), C [3,4), A [4,9). At t=2, B and C each have one unit left, so the tie rule keeps B running.

| Job | First run | Completion | Turnaround | Response |
| --- | ---: | ---: | ---: | ---: |
| A | 0 | 9 | 9 | 0 |
| B | 1 | 3 | 2 | 0 |
| C | 3 | 4 | 2 | 1 |

Mean turnaround = 13/3; mean response = 1/3. See [[review/virtualization/cpu-scheduling|scheduling metrics]].

## 2. Address translation

256-byte pages give 8 offset bits. VPN=`0x12`, offset=`0xAB`, PFN=`0x34`, so PA=`0x34AB`. There are 2^(16-8)=256 virtual pages. A full linear table with 4-byte PTEs takes 1024 bytes. See [[review/virtualization/address-translation-and-page-tables|translation]].

## 3. TLB locality

Use least-to-most-recent order:

| Reference | Outcome | TLB afterward |
| --- | --- | --- |
| A | Miss | A |
| B | Miss | A, B |
| A | Hit | B, A |
| C | Miss, evict B | A, C |
| B | Miss, evict A | C, B |
| A | Miss, evict C | B, A |

Five misses in six lookups. All pages remain resident; no disk faults are implied. See [[review/virtualization/tlb-and-locality|TLB locality]].

## 4. Page replacement

FIFO has five misses: A, B, C, D, then the final A. LRU has four: A, B, C, D. FIFO evicts oldest-resident A for D despite the earlier hit. LRU evicts B because the hit made A recent. See [[review/virtualization/page-replacement|replacement]].

## 5. Condition variables

A signaled consumer can reacquire the mutex after another consumer already removed the available item. Spurious wakeups are also allowed. Use `while (count==0)` and recheck the predicate under the mutex. `cond_wait()` atomically releases the held mutex while waiting and reacquires it before returning. See [[review/concurrency/condition-variables|condition variables]].

## 6. Semaphores

Initialize empty=5, full=0, mutex=1. Producer order: wait(empty), wait(mutex), insert and update shared indices/count, post(mutex), post(full). Blocking for an empty slot while holding the sole buffer mutex can prevent consumers from creating that slot. See [[review/concurrency/semaphores|semaphores]].

## 7. Deadlock

T1 takes A; T2 takes B; T3 takes C; T1 waits for B; T2 waits for C; T3 waits for A. The held/waiting graph cycles through all three. A global order A<B<C prevents circular wait if all threads obey it: T3 must acquire A before C. See [[review/concurrency/deadlocks-and-progress|deadlocks]].

## 8. Disk latency

One revolution = 60000/12000=5 ms; mean rotation=2.5 ms. Transfer = (8192/(100*2^20))*1000=0.078125 ms. Total modeled latency=3+2.5+0.078125=5.578125 ms. These are estimates under a random-position model, not a measured service-time guarantee. See [[review/persistence/hard-disks-and-scheduling|disk latency]].

## 9. RAID

RAID 0 disk=19 mod 4=3; per-disk block offset=floor(19/4)=4. Five-disk RAID 5 usable capacity=(5-1)*2=8 TiB; it tolerates one arbitrary disk failure in the stated model. RAID-5 physical mapping was not asked and would need a parity-rotation convention. See [[review/persistence/raid|RAID]].

## 10. Names and inodes

Only name c remains among a, b, c. It contains XY. Before unlink, b and c name the same inode, so appending via b changes what c reads. Unlinking b removes its directory entry but c retains a link. See [[review/persistence/files-descriptors-and-links|links]].

## 11. Path lookup

Seven reads: root inode, root data, x inode, x data, y inode, y data, z inode. No read of z's data is necessary just to load its inode. No timestamps or cache effects are included. See [[review/persistence/file-system-layout-and-traces|FS I/O traces]].

## 12. Journaling

After four writes, the header and three payloads are durable but no commit exists; recovery ignores this uncommitted transaction. After five, commit is durable and recovery can replay all three intended home updates.

Reclaiming/reusing the log before home writes persist can destroy the only durable recovery copy. A crash could then leave partial home state with no recoverable transaction. See [[review/persistence/journaling-and-crash-consistency|journal ordering]].

## 13. LFS

Live among the listed blocks: 10, 33, 35, 40. Current map 40 selects inode 35, which selects data 10 and 33. Block 10 remains live even though the older inode also referenced it. Blocks 11, 20, and 25 are unreachable from the authoritative current state under the assumptions. See [[review/persistence/log-structured-file-systems|LFS liveness]].

## 14. Flash

The map becomes 7→9; reading logical 7 returns B. Physical page 3 remains programmed but is obsolete for logical 7. Its containing block need not be erased immediately. Later garbage collection may copy other live pages and erase the block as a unit. See [[review/persistence/ssds-and-flash-translation|FTL updates]].

## 15. Checksums

XOR: `0x12 XOR 0x34=0x26`, then XOR `0x56=0x70`, then XOR `0x78=0x08`.

Sum: 18+52+86+120=276; 276 mod 256=20=`0x14`. A matching checksum does not alone prove freshness; stale data and its stale checksum may agree. See [[review/persistence/checksums-and-data-integrity|integrity]].

## 16. Virtualization and networks

The guest OS maps guest virtual to guest physical memory; the hypervisor/second-stage translation maps guest physical to host physical. A VM can run its own guest kernel; a container uses the host kernel with isolated views and resource controls.

A lost reply leaves execution uncertain to the caller. Retrying a non-idempotent operation such as "increment balance by one" can increment twice without a suitable duplicate-suppression protocol. Reliable transport alone does not establish exactly-once application execution. See [[review/advanced/virtual-machines-and-containers|VMs]] and [[review/advanced/network-reliability-and-rpc|RPC]].
