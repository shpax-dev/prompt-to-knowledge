# Checksums and Data Integrity

Storage can return a read error, corrupted bits, data written to the wrong location, or old data after a lost write. Redundancy and integrity detection solve related but different problems.

A checksum summarizes data. On read, recompute and compare it with the stored expected value. Simple XOR and addition checksums are easy to calculate but weak: different inputs can collide.

## Original practice

For bytes `0x12`, `0x34`, `0x56`, `0x78`:

```text
XOR: 0x12 XOR 0x34 XOR 0x56 XOR 0x78 = 0x08
sum: (0x12 + 0x34 + 0x56 + 0x78) mod 256 = 0x14
```

A one-byte addition checksum ignores overflow only because the exercise says to reduce modulo 256. A real checksum algorithm's arithmetic must be specified.

## What is the expected value bound to?

- A data checksum can detect some changed contents.
- Including the expected block identity can help detect misdirected writes.
- A stale data block and its equally stale adjacent checksum can still agree after a lost write. Independent expected-checksum placement, versions, or another appropriately designed protocol can expose that problem.

With two disagreeing mirror copies, duplication alone does not identify which one is correct. A checksum only helps if its expected value and associated identity/version are trustworthy. Do not equate parity with proof of freshness or authenticity.

Source: `FA23 - exam.pdf`, pp. 3 and 9-10, questions 29-30 and 55-57. The numerical example above is newly generated. Related: [[review/persistence/raid|RAID]] and [[review/persistence/journaling-and-crash-consistency|crash recovery]].
