# Persistence and Final Preparation

The supplied material emphasizes persistence, with broader topics in some final exams. The FA24 paper adds virtual machines, multiprocessor scheduling, networking, and Project 6 questions; do not infer identical coverage for another offering.

## Solve a chain of mechanisms

- Begin with [[review/persistence/io-devices-and-dma|I/O completion and data movement]], then calculate [[review/persistence/hard-disks-and-scheduling|disk latency, capacity, and request order]].
- For [[review/persistence/raid|RAID]], identify the layout and workload before calculating capacity, operations, latency, or throughput.
- Separate [[review/persistence/files-descriptors-and-links|names, inodes, and open-file descriptions]], then trace [[review/persistence/file-system-layout-and-traces|file-system operations]] and [[review/persistence/file-allocation-and-indexing|indirect indexing]].
- Explain [[review/persistence/fast-file-system|FFS locality]], [[review/persistence/journaling-and-crash-consistency|journal ordering]], and [[review/persistence/log-structured-file-systems|LFS reachability]] as different mechanisms.
- Trace [[review/persistence/ssds-and-flash-translation|FTL mappings]] and reason about [[review/persistence/checksums-and-data-integrity|integrity checks]].
- Review [[review/advanced/network-reliability-and-rpc|network reliability]], [[review/advanced/distributed-file-systems|NFSv2/AFS models]], [[review/advanced/virtual-machines-and-containers|VMs/containers]], and [[review/advanced/multiprocessor-scheduling|multiple CPUs]] where the chosen paper requires them.

## Practice sequence

First solve small untimed traces with explicit state. Next attempt one section of a historical exam without solutions. Review the mechanism behind errors, then retry a fresh problem. Only simulate a full timed exam when using a specific paper's rules or the user's confirmed current policy.

The [[exam-prep/past-exams/fa15/fa15-persistence|FA15]], [[exam-prep/past-exams/fa19/fa19-persistence-final|FA19]], [[exam-prep/past-exams/fa23/fa23-persistence|FA23]], and [[exam-prep/past-exams/fa24/fa24-final|FA24]] notes keep formats and coverage separate. Historical frequency is evidence of practice value, not a prediction of future question weight.

For an allowed note sheet, prioritize formulas with units, translation fields, synchronization invariants, and write-order dependencies. Confirm whether a sheet is permitted for the actual exam before using a historical allowance. No calendar, deadline, or personal progress is assumed.

Source: `CS537 - Final Review.pdf`, pp. 1-73, plus the four separately supplied historical papers. Work [[exam-prep/practice/operating-systems-practice|original practice questions]] 8-16 before reading their derived solutions.
