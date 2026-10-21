# Locks and Atomic Primitives

A lock serializes access to a critical section. It protects the agreed shared invariant only when every relevant access follows the same protocol. Acquiring a user-level lock does not prevent the scheduler from preempting the holder.

## Goals

- **Mutual exclusion:** at most one owner enters the protected critical section.
- **Progress:** contention does not permanently prevent all useful progress.
- **Fairness/starvation freedom:** an individual waiter eventually gets service under stated scheduling assumptions.
- **Efficiency:** reasonable CPU, scheduling, and cache-coherence costs.

Fairness does not mean all threads wait exactly the same time.

## Why test then set fails

Two threads can both read an ordinary `flag == 0` before either writes 1. An atomic read-modify-write combines the decision and update into one indivisible operation.

**Teaching pseudocode, not a complete C implementation:**

```text
initial flag = 0                  # free
acquire:
    while atomic_exchange(flag, 1) == 1:
        spin
release:
    atomic_store_release(flag, 0)
```

The successful exchange sees the old value zero. Correct production code also needs appropriate acquire/release memory ordering. Some exam questions reverse the meaning of zero and one; derive the loop from the supplied convention rather than memorizing the snippet.

Compare-and-swap conditionally replaces an expected value. Its return convention depends on the API. Ticket locks use an atomic fetch-and-add to assign tickets and wait until a serving counter equals the ticket. FIFO admission does not eliminate scheduling assumptions or waiting cost.

## Spin, yield, or block

Spinning can be effective when a lock is held briefly on another running CPU. Blocking avoids burning CPU while waiting for a long critical section but has scheduling overhead. Yielding relinquishes execution without necessarily removing a thread from the runnable set.

On one CPU, spinning can waste a quantum while the lock holder cannot run; kernel rules and preemption determine whether it can also deadlock. Avoid absolute claims that every spinlock on one CPU is impossible.

A queued blocking implementation must close the race between joining the wait queue and actually sleeping. An early `unpark()` must not be lost before the waiter parks. The precise handshake depends on the primitive. Two-phase waiting spins for a bounded interval before blocking; it is a tradeoff, not a prediction oracle.

Source: `CS537 - Exam 2 Review.pdf`, PDF pp. 5-18, 37-39, and 88-92. Related: [[review/concurrency/condition-variables|predicate waiting]] and [[review/concurrency/deadlocks-and-progress|progress failures]].
