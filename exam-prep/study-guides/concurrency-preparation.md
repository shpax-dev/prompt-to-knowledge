# Concurrency Preparation

This guide follows `CS537 - Exam 2 Review.pdf`. Concurrency questions test which schedules are possible and which invariants survive all allowed schedules.

## Work from shared state to ordering

1. Identify [[review/concurrency/threads-and-races|thread-local and shared state]]. Split compound updates into teaching-model load/modify/store steps when the question calls for instruction interleavings.
2. For [[review/concurrency/locks-and-atomic-primitives|locks]], write down free/held values, atomic return semantics, ownership, runnable threads, and queued sleepers.
3. For [[review/concurrency/condition-variables|condition variables]], name the predicate, its mutex, and the correct wait queue. Explain atomic release/wait and reacquisition.
4. For [[review/concurrency/semaphores|semaphores]], initialize counts from available resources or required event ordering. Track reserved permits as well as completed work.
5. For [[review/concurrency/deadlocks-and-progress|deadlocks]], build a full held/waiting graph. Distinguish one bad schedule from a proof of unavoidable failure.

## Trace table

Use columns for step, executing thread, next instruction, held locks, semaphore permits, shared values, and runnable/blocked state. A scheduler choice cannot run a blocked thread without a wakeup. A signal does not give the waiter the mutex immediately.

The review includes lock construction, producer/consumer, thread-join, linked-list insertion, semaphore sequencing, and reader/writer traces. Useful printed exercise ranges include pp. 37-45, 49-53, 94-107, and 109-120.

## Check the artifact, not just the filename

The collection embeds Fall 2024 Midterm Exam #2 and Spring 2024 Midterm Exam #2 material with different allowed-material policies. The Fall cover at PDF p. 93 allows one sheet of notes and states 90 minutes; the Spring cover at p. 108 says closed notes and 90 minutes. These are historical policies for different exams, not a contradiction requiring a combined rule.

Use [[exam-prep/practice/operating-systems-practice|original practice questions]] 5-7. For C examples with unsynchronized conflicting accesses, distinguish the exam's simplified interleaving model from actual language-level undefined behavior. Do not present a casual interleaving as a portable C guarantee.

Source: `CS537 - Exam 2 Review.pdf`, pp. 1-120. Handwritten annotations are study aids; visually verify them and derive a schedule before treating them as a grading key.
