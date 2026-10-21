---
date-generated: 2026-10-20
date-updated: 2026-10-20
---

# File-System Layout and Traces

A simple inode-based file system has a superblock, inode allocation bitmap, data allocation bitmap, inode table, and data blocks. Directories store name-to-inode mappings inside their data blocks. A bitmap allocation bit and a pointer to an object must agree.

## Trace objects, not just filenames

For each operation, separately track directory entries, inode metadata/link counts, data pointers, allocated blocks, and relevant caches. A new empty regular file need not allocate a data block. Adding a hard link allocates a name, not a new inode or a new copy of the contents.

The supplied teaching simulators do not all use identical directory reference-count conventions. Read that particular exercise's definition and initial state. Do not infer universal POSIX directory link counts from a simulator that counts additional references.

## Original practice: cold pathname lookup

Assume each inode occupies its own block, each directory uses one data block, nothing is cached, all components exist, and timestamps are ignored. To open `/a/b` and load b's inode:

```text
read root inode
read root directory data -> find a's inode number
read a inode
read a directory data -> find b's inode number
read b inode
```

That is five block reads. The file's contents have not been read yet. If the root inode were cached, the count would change. For a three-component path under the same assumptions, the count is seven reads.

## Mutation accounting

Creating an empty file usually updates an inode bitmap, the new inode, parent directory data, and relevant parent metadata. Appending a newly allocated data block involves allocation metadata, the block contents, and the file inode. Exact read/write counts depend on block packing, cache state, partial-block preservation, directory growth, timestamp updates, and journaling. Build the access list before counting; do not memorize one universal number.

When reading a before/after snapshot, identify which names appeared or disappeared, which inode numbers are shared, which pointer changed, and whether a bitmap or reference count also changed.

Source: `CS537 - Final Review.pdf`, PDF pp. 25-28; `FA15 - exam.pdf`, pp. 3-5; `FA19 - exam.pdf`, pp. 3-7; `FA23 - exam.pdf`, pp. 5-7; `FA24 - exam.pdf`, pp. 7-9.
