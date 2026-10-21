# RAID

RAID combines disks behind one logical block interface. Keep usable capacity, tolerated failures, per-request latency, and steady-state throughput separate. Assume N equal disks of capacity C, random request service rate R, and sequential rate S unless a question says otherwise.

| Layout | Usable capacity | Failure behavior | Key mechanism |
| --- | --- | --- | --- |
| RAID 0 | N*C | No disk failure tolerated | Striping |
| Two-way mirrored pairs, often RAID 10 for N>2 | (N/2)*C | Any one failure; multiple failures only when each pair retains a copy | Mirroring and optionally striping across pairs |
| RAID 4 | (N-1)*C | One disk failure | Dedicated parity disk |
| RAID 5 | (N-1)*C | One disk failure | Distributed parity |

The review sometimes uses RAID 1 for its N-disk mirrored-pair model. State that model: a general N-way mirror of all data instead has capacity C. Layout terminology must not silently change the arithmetic.

## Address mapping

For RAID 0 with one-block chunks and logical block L:

```text
disk = L mod N
offset on disk = floor(L/N)
```

For larger chunks, divide into chunk number and within-chunk offset first. Mirrored-pair numbering and RAID-5 parity rotation must be supplied by the layout diagram or rule. "Left symmetric" still needs a precise numbering convention when matching an exam diagram.

## Parity updates

```text
P = D0 XOR D1 XOR ...
P_new = P_old XOR D_old XOR D_new
```

A small read-modify-write update reads old data and parity, then writes new data and parity: four disk operations. The two reads can overlap on separate disks; so can the two writes. This differs from a full-stripe write, which can compute parity from all new data without first reading old parity.

In the simplified balanced independent-request model, RAID 5 small random-write throughput is roughly N*R/4. RAID 4's parity disk handles a read and a write per logical update, limiting throughput to about R/2. Ideal sequential full-stripe throughput is roughly (N-1)*S. These are model results, not guarantees for all queue depths, controllers, degraded states, or request sizes.

Mirroring/parity alone cannot identify the correct copy after arbitrary silent corruption and is not a backup. Interrupted multi-disk writes need crash-consistency handling.

Source: `CS537 - Final Review.pdf`, PDF pp. 8-15; `FA15 - exam.pdf`, pp. 2-3; `FA19 - exam.pdf`, p. 12; `FA23 - exam.pdf`, p. 2. Related: [[review/persistence/checksums-and-data-integrity|integrity]].
