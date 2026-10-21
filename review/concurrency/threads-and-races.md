# Threads and Races

Threads in one process share its address space, including code, heap, and accessible stack memory. Each thread has its own logical execution context: program counter, register values, stack pointer, and stack. A stack's ownership by one thread is not memory isolation from other threads in the same process.

The process also supplies shared resources such as file descriptors. Switching between threads in one address space ordinarily avoids switching to a different process's page table; exact kernel mechanisms vary.

## Thread models

In a many-to-one user-level model, a runtime schedules multiple logical threads on one kernel scheduling entity. A blocking syscall can block the whole entity and those logical threads cannot run simultaneously on multiple CPUs. A one-to-one kernel-thread model allows independent scheduling and blocking. These describe models; user-space runtimes are not universally obsolete.

`pthread_create()` does not inherently wait for the created thread to finish. `pthread_join()` waits for the specified thread and supports safe observation of its completed work. A thread's return values, automatic variables, and shared data must be distinguished when tracing code.

## Original practice: a lost update

In a simplified instruction-interleaving model, incrementing shared x means load, add, store. Start x=0:

```text
T1 load x -> r1=0
T2 load x -> r2=0
T1 add 1  -> r1=1
T2 add 1  -> r2=1
T1 store r1 -> x=1
T2 store r2 -> x=1
```

Two increments leave one visible update. Each thread has its own register state even when both use a register with the same architectural name.

This is a teaching trace. Unsynchronized conflicting access to an ordinary C/C++ object is a language-level data race and has undefined behavior; the trace is not a portable list of possible C outcomes. `volatile` does not make an operation atomic or create synchronization.

Use [[review/concurrency/locks-and-atomic-primitives|locks]] or appropriate language-level atomics. Adding threads does not guarantee linear speedup: serial work, contention, scheduling, and communication matter.

Source: `CS537 - Exam 2 Review.pdf`, PDF pp. 1-10 and embedded thread exercises, pp. 94-97 and 109-114.
