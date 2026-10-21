---
date-generated: 2026-10-20
date-updated: 2026-10-20
---

# Processes and Limited Direct Execution

A program is passive code and data. A process is an executing instance with an address space, registers, a program counter, and OS-managed state. The OS multiplexes the CPU while preserving the illusion that each process has its own execution environment.

## Limited direct execution

Run ordinary application instructions directly on the CPU in user mode. Restrict operations that could bypass isolation, such as changing memory-management state or controlling devices. A system call deliberately transfers control into the kernel; a timer interrupt allows the OS to regain control without waiting for a cooperative program.

A trap or interrupt changes execution mode and transfers control to a configured handler. A **mode switch** does not necessarily select a different process. The handler may return to the same process. A **context switch** saves one execution context and restores another.

The PCB records scheduling and process metadata and locates saved execution state. A kernel stack supports kernel execution on behalf of a process or thread. The exact split between PCB, trap frame, and kernel stack is implementation-specific; do not assume every register is stored in one universal structure.

## Process states

- **Running:** executing on a CPU.
- **Ready:** can execute but is waiting to be scheduled.
- **Blocked:** waiting for an event such as I/O completion.

If A blocks for I/O, B can run. I/O completion typically makes A ready; it does not guarantee A immediately runs. See [[review/virtualization/cpu-scheduling|CPU scheduling]].

## Process API

- `fork()` creates a child. On success, the parent receives the child's PID and the child receives zero. Failure returns a negative value to the caller.
- The child resumes after `fork()`, not at `main()`.
- Successful `exec()` replaces the calling process's program image; it does not create a second process and does not return to the old program. The PID remains the same.
- `wait()`/`waitpid()` can collect a child's status; the precise stopping and termination behavior depends on the API and options.

**Original practice:** if a single process executes two successful, unconditional `fork()` calls and all resulting processes reach both calls, there are four processes after the second call. Draw the tree before reasoning about printed output. Output order and stdio buffering require additional assumptions.

Source: `CS537 Exam 1 Review.pdf`, PDF pp. 1-9 and 13; embedded virtualization exam material, pp. 96-100. For shared initial memory after fork, see [[review/virtualization/demand-paging-and-working-sets|copy-on-write]].
