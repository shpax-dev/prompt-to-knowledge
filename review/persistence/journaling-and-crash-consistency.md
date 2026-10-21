---
date-generated: 2026-10-20
date-updated: 2026-10-20
---

# Journaling and Crash Consistency

A single logical file operation can change several on-disk structures. A crash between writes can leave a pointer to an unallocated block, an allocated but unreachable object, or inconsistent reference counts. Device buffering and write reordering make issue order alone insufficient.

## FSCK

A checker cross-validates allocation bitmaps, inode pointers, directory references, link counts, and structural rules. It can repair a consistent structure but cannot generally recover the precise user intent or guarantee the state before the interrupted operation. A scan of a large file system is expensive.

Multiple directory entries to one inode can be valid hard links. Two distinct ordinary file inodes sharing a data block is a different question and depends on whether the model supports intentional sharing.

## Write-ahead journaling

In the simple redo-journal model:

1. Persist a transaction header and the blocks to replay.
2. Persist its commit record after the required transaction contents are durable.
3. Checkpoint: write the committed updates to their home locations.
4. Reclaim journal space only when those home writes are durable and the log protocol allows reuse.

Recovery ignores an incomplete transaction and replays an identified complete committed transaction as needed. Checkpointing may be interrupted safely because the committed journal remains available. A commit record must not become durable before the data it certifies. Home updates must not precede durable commit in the full-data redo model.

## Journal modes

- **Full data journaling:** journal both data and metadata, then checkpoint them.
- **Ordered metadata journaling:** journal metadata, and make the associated data durable at home before committing metadata that exposes it.
- **Writeback metadata journaling:** does not enforce that data-before-commit order; structurally valid metadata can expose stale contents after a crash.

These are simplified modes; they do not promise every application-level multi-file update is atomic. Ordered mode does not require all data to be durable before any journal payload write; the key dependency is before the relevant metadata commit.

## Original practice: four updated blocks

Assume one header, four payload blocks, one commit block; each completes durably in sequence and home writes start only afterward.

- After five writes: header + four payloads, no commit. Do not replay this transaction.
- After six writes: commit is durable. Replay can recover all four intended updates.

Changing the payload count, barriers, atomicity, or journal-reuse state changes the answer. The FA15/FA23/FA24 create-file questions must be interpreted using each question's stated block count, not another paper's hint.

Source: `CS537 - Final Review.pdf`, PDF pp. 31-38; `FA15 - exam.pdf`, pp. 6-8; `FA19 - exam.pdf`, p. 8; `FA23 - exam.pdf`, pp. 7-9; `FA24 - exam.pdf`, p. 9.
