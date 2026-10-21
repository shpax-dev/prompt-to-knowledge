---
date-generated: 2026-10-20
date-updated: 2026-10-20
---

# Address Translation and Page Tables

Virtual addresses identify locations in a process's address space. Physical addresses identify locations in RAM. Two processes can use the same virtual address and map it to different physical memory.

## Base and bounds

In the simple dynamic-relocation model, check the virtual byte offset before adding the physical base:

```text
valid when 0 <= VA < bound         # bound is the region length
PA = base + VA
```

Some exercises use an inclusive upper limit instead of a length. Read the stated convention before testing the boundary. Only privileged code may change the translation registers. Relocation permits moving a region by changing its physical base, but a single contiguous allocation can suffer external fragmentation.

**Original practice:** base `0x4000`, bound length `0x1000`, VA `0x02A5` gives PA `0x42A5`. VA `0x1000` is outside the region.

## Segmentation

Split the VA into segment identifier and offset. Each segment has its own base, bounds, and permissions. Independently placed code, heap, and stack regions support sparse address spaces and selective sharing. Each individual segment still needs contiguous physical space in the pure segmentation model. Downward-growing segments require the exercise's offset convention; do not apply an unsigned base-plus-offset formula blindly.

## Paging

Divide virtual memory into pages and physical memory into equally sized frames. A per-address-space page table maps VPNs to PFNs/PPNs. The byte offset is unchanged by translation.

```text
page size = 2^p bytes
VPN       = VA >> p
offset    = VA mod 2^p
PA        = PFN * 2^p + offset
```

VPN and PFN widths can differ. The low bits are the **offset**, not the frame number. Fixed-size frame allocation avoids external fragmentation at that allocation granularity; partially used pages cause internal fragmentation.

A PTE may encode mapping validity, present/resident state, permissions, accessed/reference state, and dirty state. Bit encodings are architecture-specific. A legal but nonresident mapping differs from an illegal address or a permission violation. See [[review/virtualization/demand-paging-and-working-sets|demand paging]].

## Page-table size and levels

For v VA bits, p offset bits, and e bytes per PTE:

```text
linear page table bytes = 2^(v-p) * e
entries per page-table page = 2^p / e
index bits per full level = log2(2^p / e)
```

**Derived example from the review's parameters:** 32-bit VAs, 4 KiB pages, and 4-byte PTEs give 20 VPN bits, 2^20 entries, and 4 MiB for a full linear page table. A 4 KiB table page holds 1024 PTEs. A conventional two-level split is 10 directory bits, 10 table bits, and 12 offset bits.

Multilevel tables allocate lower-level tables for populated regions. Count the root plus the distinct lower-level table pages actually needed; do not charge one table page per data page. With n levels and no TLB or walk cache, translation takes n PTE memory reads, then one additional access for the requested data. Faults may terminate the walk early.

Source: `CS537 Exam 1 Review.pdf`, PDF pp. 15-37, 45, and 52-53; multilevel exercise and solution material, pp. 108-113. The arithmetic above is independently derived. [[review/virtualization/tlb-and-locality|TLBs]] cache completed translations.
