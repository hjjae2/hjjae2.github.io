---
type: 'docs'
bookCollapseSection: false
bookHidden: false
bookToC: true
bookComments: false
bookSearchExclude: false
bookFlatSection: true
weight: 1
---

# What is eBPF?

eBPF is a technology with origins in the Linux kernel that **can run sandboxed programs** in a privileged context such as the OS kernel. **It is used to 
safely and efficiently extend the capabilities of the kernel without requiring to change kernel code or load kernel modules.**

## What do eBPF and BPF stand for?

BPF originally stood for **Berkeley Packet Filter**, but now that eBPF(experimental BPF) can do so much more thant packet filtering. In the Linux 
source code, the term BPF persists, and in tooling and in documentation, the term BPF and eBPF are generally used interchangeably. The original 
BPF is sometimes referred to as "classic BPF" or "cBPF".

# Introduction to eBPF

## Hook Overview

eBPF programs are event-driven and are run when the kernel or an application passes a certain hook point.

Pre-defined hooks include **system calls, function entry/exit, kernel tracepoints, network events, and several others.**

If a predefined hook dose not exist for particular need, it is possible to create a kernel probe(kprobe) or user probe(uprobe) to attach eBPF 
programs almost anywhere in kernel or user applications.

## How are ePBF programs written?

In a lot of scenarios, **eBPF is not used directly but indirectly via projects like Cilium, bcc, bpftrace** which provide an abstraction on top of 
eBPF and do not require writing programs directly but instead offer the ability to specify intent-based definitions.

# Reference
https://ebpf.io/what-is-ebpf