# Review Agent Instructions

Read the root `AGENTS.md` first. This file governs explanations and concept documentation under `review/`.

## Teaching and retrieval

Find and read the canonical concept note and its cited source before making a source-specific claim. Begin with the mechanism, then explain why it exists and its tradeoffs. Use a small trace or calculation when it reveals behavior better than prose. Link related concepts in context.

For questions, answer in chat without writing notes. For an explicit documentation request, update the focused concept note. Append only genuinely new information; merge repeated agent-generated explanation into the existing section when appropriate. Preserve user-authored observations.

## Technical conventions

- State the machine and workload model: single CPU versus multiple CPUs, preemptive versus nonpreemptive scheduling, page size, cache state, disk geometry, and relevant failure assumptions.
- Separate virtual addresses, physical addresses, page numbers, byte offsets, block numbers, and inode numbers. Keep units on calculations.
- Distinguish a TLB miss, an invalid mapping, a not-present page, and a protection fault.
- Treat concurrency snippets as teaching pseudocode unless they explicitly use valid language-level atomics and synchronization. A C/C++ data race is not a portable arithmetic interleaving experiment.
- For file-system traces, distinguish pathnames, directory entries, inodes, open-file descriptions, and data blocks. Apply the source simulator's directory-reference convention only to that simulator.
- For journaling, state what must be durable before commit, checkpoint, and journal reuse. Do not equate a completed syscall or `close()` with power-loss durability.
- For RAID, state layout, disk count, chunk size, request pattern, and whether calculating latency or throughput. Do not assume a RAID-5 parity rotation from its name alone.
- Label scheduler and network models as historical where appropriate. Do not present historical Linux CFS or NFSv2 course models as a current implementation audit.

## Working with the supplied PDFs

Handwriting and diagrams may require visual inspection. OCR is an aid, not authority for code, addresses, arrows, exponents, or answers marked by color. Duplicate or annotated pages do not constitute separate independent evidence. Use 1-based PDF page numbers and preserve the source year.

A useful concept note usually contains a concise explanation, key mechanism or formula, an example when helpful, a specific pitfall, and source provenance. Include only sections that add information. Do not create empty templates or full copied exam solution sets.

## Validation

Check arithmetic independently. Trace each table entry and code transition. If an apparent source mistake changes the explanation or answer, ask about it before editing the affected claim. Do not silently "correct" source annotations or erase a student's reasoning. Report unreadable material and absent specifications explicitly.
