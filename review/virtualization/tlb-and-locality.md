# TLB and Locality

The translation lookaside buffer caches virtual-to-physical translations. A TLB hit avoids walking the page table for that access; it does not imply the requested data is in a CPU data cache.

## Hit and miss reasoning

```text
miss rate = translation misses / translation lookups
hit rate  = 1 - miss rate
TLB reach = number of entries * page size       # equal-sized pages
```

A TLB miss can be satisfied by a resident page's PTE without disk I/O. A page fault involves the mapping, permissions, or residency and may require OS work. Hardware-managed TLBs walk a defined page-table format; software-managed ones transfer miss handling to privileged software.

Context switches must prevent a new address space from using the previous one's translations. Approaches include invalidating relevant TLB entries or tagging them with address-space identifiers. Invalidating TLB entries does **not** invalidate the process's page-table mappings.

## Original practice: a sequential array

Assume an initially empty TLB, page-aligned array of 1024 four-byte integers, 4 KiB pages, one array access per element, and no interference or instruction references.

All elements occupy one page. The first access misses and the remaining 1023 hit: miss rate = 1/1024. If instead the array spans many pages and each is visited only once in order, expect one compulsory miss per visited page. More TLB entries need not improve that single-pass pattern.

For a cyclic access to k distinct pages with an LRU TLB containing fewer than k entries, every access can miss. Random replacement can sometimes outperform LRU on that particular pattern; LRU is not universally best.

Larger pages increase reach per entry but can increase internal fragmentation and allocation constraints. Arrays often have stronger spatial locality than scattered linked structures, though actual layout determines behavior.

Source: `CS537 Exam 1 Review.pdf`, PDF pp. 32-37, 44, 52-53, 71, and 74. For replacement algorithms acting on physical pages rather than translations, see [[review/virtualization/page-replacement|page replacement]].
