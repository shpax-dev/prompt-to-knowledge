---
date-generated: 2026-10-20
date-updated: 2026-10-20
---

# I/O Devices and DMA

The OS uses a device interface while a device driver translates generic requests into device-specific operations. A controller may have its own processor or firmware, but OS driver code and controller firmware are different layers.

## Device interface

A canonical teaching device exposes status, command/control, and data registers. The CPU can access device registers through specialized I/O instructions or memory-mapped I/O, depending on the architecture. Mapping registers into an address range does not turn them into ordinary RAM.

Hierarchical buses connect components with different bandwidth and latency needs. These are architectural models; numerical rates depend on the actual system.

## Two independent decisions

| Question | Options | Tradeoff |
| --- | --- | --- |
| How do we detect completion? | Polling versus interrupts | Polling can waste CPU but avoid interrupt overhead for very fast completion |
| Who moves the data? | Programmed I/O versus DMA | CPU copies versus a device/controller transferring to or from memory |

With DMA, the CPU still sets up the transfer and handles coordination/completion. DMA reduces CPU copying; it does not eliminate every CPU action. Polling and DMA can coexist, as can interrupts and programmed I/O.

An interrupt flood can consume so much processing that useful work stops progressing. Coalescing events, batching, or hybrid polling/interrupt strategies trade latency against overhead. "Interrupts always beat polling" is too strong.

Source: `CS537 - Final Review.pdf`, PDF pp. 1-4; `FA24 - exam.pdf`, pp. 2-3, questions 1 and 31. Related: [[review/persistence/hard-disks-and-scheduling|disk latency]].
