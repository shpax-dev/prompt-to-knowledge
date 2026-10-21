# Multiprocessor Scheduling

Multiple CPUs introduce a tradeoff between balancing runnable work and preserving cache affinity. Moving a thread to a different CPU can improve utilization but lose warm cache state and incur synchronization or migration costs.

## Queue designs

A single shared run queue offers a straightforward global view but needs coordination when CPUs select or update jobs. A shared queue does not eliminate locking overhead.

Multiple per-CPU queues reduce some shared contention and improve affinity, but work can become unbalanced. Work stealing or another balancing policy moves work from a busy queue to an idle or underloaded CPU.

## Historical scheduler questions

The FA24 exam asks about nice values, CFS virtual runtime, ULE, and queue classifications. In the classic CFS course model, choose the runnable task with the smallest virtual runtime; weights affect how virtual runtime accrues, and lower nice values receive greater scheduling weight.

These are historical study concepts, not an audit of present Linux scheduling behavior. Exact ULE queue selection and project-era scheduler classifications need the corresponding lecture material before making a source-specific answer key. The supplied final review does not explain every scheduler detail tested in FA24.

## Original practice

CPU 0 has ten runnable jobs while CPU 1 has none. A strictly local queue policy can leave CPU 1 idle. Stealing one or more jobs improves utilization, but the decision may trade away cache locality. A complete scheduling problem needs job affinities, priorities, migration rules, and overhead assumptions.

Source: `FA24 - exam.pdf`, pp. 2-3 and 13, questions 23-27 and 51. Related: [[review/virtualization/cpu-scheduling|single-CPU scheduling]].
