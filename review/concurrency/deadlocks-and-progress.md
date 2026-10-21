# Deadlocks and Progress

Deadlock is a set of threads waiting indefinitely for resources or events that only the blocked set can release or produce. Distinguish it from starvation (one thread is repeatedly denied progress) and livelock (threads keep acting but useful work does not complete).

## Coffman conditions

For the conventional resource model, deadlock requires:

1. Mutual exclusion for at least some resources.
2. Hold-and-wait: retaining one resource while waiting for another.
3. No preemption of the relevant resources.
4. Circular wait among participants.

For single-instance resources, a wait-for cycle demonstrates deadlock in the shown state. With multiple instances, a simple cycle need not prove deadlock; consider available counts and remaining demands.

## Prevention and tradeoffs

A consistent total lock order removes circular wait: assign ranks and acquire in increasing rank order. The order must hold across all code paths, including helper calls. "Each thread uses some consistent order" is insufficient if different threads use incompatible orders.

Acquire-all-or-none protocols remove hold-and-wait but need advance knowledge and careful implementation. A try-lock loop that releases held locks on failure can avoid a particular deadlock while still allowing livelock or starvation. Backoff reduces repeated collisions; it is not a universal progress proof.

Replacing a mutex with a CAS retry loop does not automatically make an algorithm wait-free. **Lock-free** means system-wide progress under the relevant model; **wait-free** requires every operation to complete in bounded steps. An individual CAS loop can repeatedly lose.

## Original practice

T1 acquires A then B. T2 acquires B then A. Both begin with no locks held.

```text
T1 acquires A
T2 acquires B
T1 waits for B
T2 waits for A
```

The final state has a cycle. Requiring both to acquire A before B prevents this cycle. Neither the existence of a safe schedule nor a single successful run proves the original program deadlock-free.

For a longer trace, list held locks and outstanding requests for every thread. Analyze the complete graph; pairwise comparisons alone can miss a three-thread cycle.

Source: `CS537 - Exam 2 Review.pdf`, PDF pp. 25-28, 60-63, 100-102, and 117-120. General explanations here are not an endorsement of every handwritten solution annotation in that collection.
