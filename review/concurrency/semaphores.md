---
date-generated: 2026-10-20
date-updated: 2026-10-20
---

# Semaphores

A semaphore tracks available permits and provides atomic wait/post operations. Unlike a condition-variable notification, a posted permit can be consumed later even if nobody was waiting when it was posted.

In an abstract permits model, `wait()` blocks until a permit is available and then consumes it; `post()` returns or creates a permit and may wake a waiter. Some course implementations describe a negative internal counter as the number of waiters. Do not assume this internal representation is a portable user-visible value.

## Initialization depends on the invariant

| Purpose | Initial permits | Protocol |
| --- | ---: | --- |
| Mutual exclusion | 1 | Wait before entry; post after exit |
| One-time completion/order | 0 | Worker posts on completion; observer waits |
| Pool of k interchangeable resources | k | Consume before use; return after use |
| Empty slots in an initially empty buffer | capacity | Producer waits; consumer posts |
| Full slots in an initially empty buffer | 0 | Consumer waits; producer posts |

**Original practice:** three identical stalls and three identical sinks can be represented by two counting semaphores initialized to 3, when only counts matter and there are no additional pairing constraints. One permit per resource category does not identify a particular stall or sink.

## Multiple-producer, multiple-consumer buffer

For capacity N, initialize `empty=N`, `full=0`, and `mutex=1`.

```text
producer:
    wait(empty)
    wait(mutex)
    insert one item; update index
    post(mutex)
    post(full)

consumer:
    wait(full)
    wait(mutex)
    remove one item; update index
    post(mutex)
    post(empty)
```

The mutex protects buffer mutation and indices. Acquire the availability permit before the mutex: blocking on an empty/full condition while holding the only mutex can stop the other side from changing that condition.

Semaphore counts are not simply equal to the physical number of empty/full slots at every intermediate line: a thread may reserve a permit before completing the corresponding buffer operation. Track reservations when tracing interleavings.

Source: `CS537 - Exam 2 Review.pdf`, PDF pp. 22-24, 45, 99-101, and 116-117. For deadlock analysis see [[review/concurrency/deadlocks-and-progress|deadlocks and progress]].
