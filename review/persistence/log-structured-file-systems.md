---
date-generated: 2026-10-20
date-updated: 2026-10-20
---

# Log-Structured File Systems

LFS collects updates and writes them to new locations in sequential segments. New versions supersede old blocks. It is not a journal that must copy every updated block back to a fixed home address.

## Finding current objects

```text
checkpoint region -> imap chunk -> current inode -> data block
```

A directory stores stable inode numbers. The inode map translates an inode number to its current disk address, so moving an inode does not require rewriting every directory that names it.

Updating data normally appends the changed data, a new inode referencing it, and the relevant updated imap piece. Unchanged data blocks can remain where they are. Writing a new inode does not require copying all the file's old data into the same segment.

A checkpoint region locates imap pieces and recovery information. Alternating checkpoint regions and validation allow recovery to choose a complete checkpoint when another update was interrupted. A newer valid log tail may also need recovery; a checkpoint is not automatically the only durable state.

## Liveness and cleaning

An inode version is current when the current imap points to its location. For a data block, the segment summary identifies the owning inode and logical offset; inspect that current inode to see whether it still points to this physical block. A summary entry by itself does not prove liveness.

Cleaning copies live blocks out of selected segments, updates pointers/maps, and reclaims whole segments for future sequential writes. It incurs extra I/O; policies balance utilization, age, and expected future change.

## Original practice: lookup I/O

Assume each object above occupies one block, nothing is cached, and no log-tail recovery is needed. Reading root directory data requires four reads: checkpoint, imap, inode, data. If the imap is already cached and usable, inode + data require two. State cache conditions before counting.

For an exam snapshot, begin from the latest authoritative checkpoint/map and follow reachability. An older inode, map piece, or data block can be garbage even if its bytes still look valid.

Source: `CS537 - Final Review.pdf`, PDF pp. 39-45; `FA15 - exam.pdf`, p. 10; `FA23 - exam.pdf`, p. 10; `FA24 - exam.pdf`, pp. 10-13.
