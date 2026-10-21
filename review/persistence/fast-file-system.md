# Fast File System

FFS improves performance by placing related objects near one another on a hard disk. It addresses the cost of scattered metadata and data as well as fragmentation as a file system ages.

## Locality and groups

Cylinder groups divide the disk into allocation regions with local metadata and free-space information. Place a file's inode near its data and keep related small files near their directory. Distribute directories or parts of large files across groups to avoid consuming all free space in one region. Large-file placement policies balance locality against space distribution.

Replicated superblock information helps recovery from metadata damage. It does not make arbitrary disk or platter failures harmless.

## Block size tradeoff

Larger blocks can improve sequential throughput and reduce per-byte bookkeeping, while increasing internal waste for small files or tails. Fragments/sub-block allocation can reduce small-file waste. The actual file-system format and policy determine the details.

FFS remains a block-indexed file system; grouping does not require every file to be one contiguous extent, and an inode may use indirect blocks for large files. Spatial locality reduces positioning cost; it does not remove the need for [[review/persistence/journaling-and-crash-consistency|crash consistency]].

## Exam reasoning

Explain the intended nearby relationship rather than memorizing "FFS puts everything together." A file system is a tree of relationships; all objects cannot simultaneously be adjacent. Distinguish the qualitative placement rule from the precise allocator's thresholds, which require the source's model.

Source: `CS537 - Final Review.pdf`, PDF pp. 28-30; `FA15 - exam.pdf`, p. 9; `FA19 - exam.pdf`, p. 13; `FA23 - exam.pdf`, p. 2. For a different write-placement strategy see [[review/persistence/log-structured-file-systems|LFS]].
