# Hard Disks and Scheduling

In the simplified geometry model, a surface contains tracks, tracks contain sectors, and a cylinder comprises tracks at the same radial position across participating surfaces. Use the exam's geometry rather than assuming it models a modern drive's physical layout exactly.

```text
sector bytes   = bytes per track / sectors per track
cylinder bytes = number of surfaces * bytes per track
capacity       = surfaces * tracks per surface * bytes per track
rotation ms    = 60000 / RPM
average rotational delay = 30000 / RPM
```

For a small uncached random I/O:

```text
latency ≈ seek + rotational delay + transfer
transfer seconds = request bytes / transfer bytes per second
```

**Original practice:** at 15,000 RPM, one revolution is 4 ms; expected random rotational delay is 2 ms. With 4 ms average seek and a 16 KiB transfer at 128 MiB/s, transfer takes 0.1220703125 ms and total modeled latency is about 6.122 ms. Sequential access can amortize positioning across many blocks. Do not pay a fresh random seek for every contiguous sector without a reason.

Always state whether MB means 10^6 bytes or the course's 2^20-byte convention. Do not mix ms and seconds in a throughput calculation.

## Request ordering

- **FCFS:** use arrival order.
- **SSTF:** choose the nearest pending track; sustained nearby arrivals can starve distant requests.
- **SCAN:** sweep in both directions, following the stated reversal rule.
- **C-SCAN:** serve in one direction, then return/wrap according to the model.
- **LOOK / C-LOOK:** reverse or wrap at request boundaries instead of necessarily reaching disk endpoints.
- **SPTF:** optimize positioning time, including rotation; this needs information often available inside the drive.

Trace request order first, then sum absolute head movements. State initial direction, endpoints, whether arrivals are simultaneous, tie-breaking, and whether return travel counts. Do not silently substitute LOOK for SCAN when endpoints are missing.

Source: `CS537 - Final Review.pdf`, PDF pp. 4-7; `FA15 - exam.pdf`, pp. 1-2; `FA23 - exam.pdf`, pp. 2-4; `FA24 - exam.pdf`, pp. 3-4.
