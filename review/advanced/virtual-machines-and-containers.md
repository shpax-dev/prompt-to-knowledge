---
date-generated: 2026-10-20
date-updated: 2026-10-20
---

# Virtual Machines and Containers

A virtual machine monitor/hypervisor presents a hardware-like environment in which a guest OS runs. It multiplexes CPU, memory, and devices while preventing the guest from directly taking over the host.

## CPU virtualization

Emulation interprets instructions in software and can support a different instruction set at substantial cost. Trap-and-emulate lets safe instructions execute directly and intercepts sensitive operations when the architecture supports the necessary traps.

For the classical Popek-Goldberg sufficient condition, **sensitive instructions must be a subset of privileged instructions**. Reversing that implication does not establish virtualizability. This concept appears as a question in the 2024 exam; a question's statement is not a source assertion that it is true.

A type-1 hypervisor runs directly on the hardware; a type-2 hypervisor runs with an underlying host OS. Paravirtualized interfaces let a cooperating guest use explicit hypervisor-aware operations, often avoiding expensive device emulation.

## Memory virtualization

```text
guest virtual -> guest physical -> host physical
```

Shadow paging maintains a combined mapping while monitoring guest translation changes. Nested hardware-assisted paging walks guest and host-stage mappings. TLBs cache translations and reduce repeated walk cost; actual access counts need the page-table levels and cache assumptions.

## Containers

Containers share the host kernel while isolating views and controlling resources. Namespaces isolate views such as processes, mounts, or networking. Control groups account for and limit groups' resources. Copy-on-write images share unchanged data. `chroot` changes pathname resolution but is not a complete container isolation mechanism by itself.

VM suspend/resume saves and restores enough virtual hardware state. Migration also transfers memory/state and arranges storage/network continuity; copying RAM alone is insufficient. Dirty-page tracking enables iterative copying, but a coherent final handoff is still needed.

Source: `CS537 - Final Review.pdf`, PDF pp. 65-73; `FA24 - exam.pdf`, pp. 2 and 13, questions 19-22 and 50. Related: [[review/virtualization/address-translation-and-page-tables|address translation]].
