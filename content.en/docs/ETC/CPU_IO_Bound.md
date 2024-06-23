---
type: 'docs'
bookCollapseSection: false
bookHidden: false
bookToC: true
bookComments: false
bookSearchExclude: false
bookFlatSection: true
weight: 1
title: 'CPU and I/O Bound'
---

# CPU Bound

**CPU-bound describes a scenario where the execution of a task or program is highly dependent on the CPU.** In a CPU-bound environment, most times, the processor(CPU) is the only component being used for execution.

Furthermore, CPU-bound operations tend to have few and long CPU bursts. CPU burst refers to the amount of time taken to execute a task, usually with the CPU. 

**As a result, it's always advisable to assign a lower priority to such tasks to prevent wasting resources.**

![/images/cpu_bound_example.png]

Applications that usually require tons of calculations are a classic example. For instance, High-Performance Computing(HPC) systems can be thought of as CPU-bound.

Due to the speed and efficiency of such systems, they are capable of computing billions of calculations per second.

# I/O Bound

**I/O-bound describes a scenario where the execution of a task or program is highly dependent on the Input/Output(I/O) operations, such as disk drives and peripheral devices.** In I/O bound environments, we wait for resources to be retrieved through the IO systems. Hence, it is safe to say that a program or task would execute faster if the I/O system worked faster.

**In addition, in executing an I/O bound task, the computing device spends its time performing I/O operations, and other resources, such as the CPU, are used less frequently or not at all.**

The I/O system refers to the I/O component of an Operating System(OS). **This is usually responsible for the communication or transfer of information between the computing device and the outside.**

It should be noted that I/O bound operations are characterized by many and fewer CPU bursts during execution.

![/images/cpu_bound_example.png]

**Due to the short CPU bursts, they are usually assigned higher priority during scheduling to ensure that system resources are better used.**

**Due to the use of the I/O system, the time spent waiting for data to be read or written can be substantial. This is considerably slower than the time it takes a CPU to complete operations.**

# How to distinguish between CPU and I/O Bound

1. We need to identify which resources make the program(task) execute faster.
2. Does the program(task) require long CPU bursts or short CPU bursts?

# References

https://www.baeldung.com/cs/cpu-io-bound