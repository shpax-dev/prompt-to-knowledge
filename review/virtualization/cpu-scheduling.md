# CPU Scheduling

Scheduling chooses among ready jobs. Keep **policy** (which job) separate from **mechanism** (timer interrupts, saved registers, dispatch, and context switching).

## Metrics

For job i with arrival A, first scheduled time S, and completion C:

```text
turnaround = C - A
response   = S - A
```

Average each job's metric; do not substitute total completion time or total service time. Response ends when a job first runs, not when it completes. Throughput counts completions per unit time.

## Policies and tradeoffs

| Policy | Decision | Main consideration |
| --- | --- | --- |
| FIFO / FCFS | Earliest ready arrival | A long job can delay short jobs: convoy effect |
| SJF | Shortest known job among ready jobs, without preemption | Optimal average turnaround for simultaneous arrivals with known lengths; later short jobs still wait for the running job |
| STCF / SRTF | Smallest remaining time, with preemption | Reconsider at arrivals and completions; assumes service-time knowledge |
| Round robin | Rotate through ready jobs using a time quantum | Small quanta can improve responsiveness but increase switching overhead |
| MLFQ | Use observed CPU behavior and priority queues | Learns behavior without knowing future job lengths |

Do not assume RR improves every workload's response or turnaround. State arrivals, quantum, tie-breaking, I/O behavior, and switching cost before tracing.

## MLFQ rules

Run the highest-priority nonempty queue; use RR among equal priorities. New jobs enter at high priority. Charge the total CPU allotment at a level even if a job repeatedly yields, so yielding just before a quantum ends does not reset its accounting. Periodic boosts mitigate starvation and adapt to phase changes. Queue counts, quanta, and boost intervals are parameters, not universal constants.

## Original practice: compare schedules

A arrives at 0 with burst 8; B arrives at 1 with burst 2. Assume one CPU, no I/O or switching cost, and no ties.

- **FIFO:** A runs [0,8), B [8,10). Turnarounds: 8, 9; average 8.5. Responses: 0, 7; average 3.5.
- **STCF:** A [0,1), B [1,3), A [3,10). Turnarounds: 10, 2; average 6. Responses: 0, 0; average 0.

Record first-run and completion timestamps independently. A lower average turnaround can coexist with worse turnaround for an individual job.

Source: `CS537 Exam 1 Review.pdf`, PDF pp. 7-14, 63-64, 79-81, and 104-105. The example above uses new inputs and independently derived arithmetic. For multi-CPU extensions see [[review/advanced/multiprocessor-scheduling|multiprocessor scheduling]].
