# Condition Variables

A condition variable lets threads sleep until shared state may satisfy a predicate. The predicate lives in ordinary shared state protected by a mutex. The condition variable is not a stored event or a substitute for that mutex.

## Correct waiting pattern

**POSIX-style teaching pseudocode:**

```c
pthread_mutex_lock(&m);
while (!ready) {
    pthread_cond_wait(&cv, &m);
}
/* use the state protected by m */
pthread_mutex_unlock(&m);
```

`pthread_cond_wait()` atomically releases the associated mutex and waits. Before a successful return it reacquires the mutex. The returning thread must recheck the predicate: another thread may consume the condition, the signal may not target the desired kind of waiter, or a spurious wakeup may occur.

A cooperating thread changes `ready` under the same mutex and signals or broadcasts according to the protocol. A signal does not force immediate execution or transfer the mutex to the waiter. A signal sent when no thread is waiting is not saved as a permit. Keeping durable predicate state prevents a late waiter from relying on a past signal.

## Bounded buffer

Use one mutex for shared indices, count, and buffer operations; `not_empty` for consumers and `not_full` for producers.

```text
producer: while count == capacity: wait(not_full, mutex)
          insert; count++
          signal(not_empty)
consumer: while count == 0: wait(not_empty, mutex)
          remove; count--
          signal(not_full)
```

Each sequence executes with the mutex held except during the wait's atomic release/sleep. Multiple producers and consumers do not imply a required equality between the number of workers and buffer slots.

One CV with `signal()` can wake a waiter whose predicate is still false while the needed waiter remains asleep. Separate CVs or a justified broadcast protocol can address the problem. `while` fixes predicate rechecking but cannot by itself guarantee the right kind of waiter is eventually notified.

Source: `CS537 - Exam 2 Review.pdf`, PDF pp. 19-21, 40-44, 64-67, and 115-116. Compare stored permits in [[review/concurrency/semaphores|semaphores]].
