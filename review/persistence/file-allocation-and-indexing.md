---
date-generated: 2026-10-20
date-updated: 2026-10-20
---

# File Allocation and Indexing

Allocation chooses where a file's blocks live; indexing finds the right physical block for a logical byte offset. Contiguous placement improves locality but makes growth and free-space management harder.

| Design | Benefit | Cost |
| --- | --- | --- |
| Contiguous allocation | Small metadata; simple indexing; good sequential access | External fragmentation and difficult growth |
| Extents | Compact records for contiguous runs | Growth can require more extents and scattered placement |
| Linked blocks | Flexible growth with fixed-size blocks | Random access traverses a chain |
| FAT | Keep chain pointers together in a table | Table space and lookup/cache costs |
| Indexed blocks | Direct indexing into pointer arrays | Index metadata and capacity limits |
| Multilevel inode indexing | Small files use direct pointers; large files add levels | Additional metadata reads on indirect paths |

Fixed-size blocks can cause internal fragmentation even when external fragmentation is avoided at the block granularity. Cached FAT entries improve lookup I/O but do not magically make a chain an O(1) array index.

## Maximum file size

Let block size be B, pointer size e, direct pointer count d, and fan-out k=B/e. With one single-, double-, and triple-indirect pointer:

```text
maximum data bytes = B * (d + k + k^2 + k^3)
```

Include only the levels actually present. Indirect blocks contain pointers, not additional file-content blocks counted by this formula. Implementation limits may impose a smaller maximum.

**Original practice:** B=4096 bytes, e=4 bytes, d=10, plus one single and one double indirect pointer. Then k=1024 and maximum data capacity is `4096*(10+1024+1024^2)` bytes. The double-indirect contribution is squared, not `2*1024`.

For a byte-offset access, first compute `logical block=floor(offset/B)` and `within-block offset=offset mod B`. Then determine whether the logical block falls in the direct, single-, or double-indirect range.

Source: `CS537 - Final Review.pdf`, PDF pp. 21-25; `FA19 - exam.pdf`, p. 12, question 78. Related: [[review/persistence/fast-file-system|physical locality]].
