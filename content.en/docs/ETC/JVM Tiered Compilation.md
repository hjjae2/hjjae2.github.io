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

# JIT Compilers

A JIT compiler compiles bytecode to native code for frequently executed sections which are called 'hotspots'.

Java can run with similar performance to a fully compiled language.

## C1 (Client Compiler)

A type of a JIT Compiler optimized for faster start-up time. It tries to optimize and compile the code as soon as possible.

It can be used for short-lived applications and applications where start-up time was an important non-functional requirement.

## C2 (Server Compiler)

A type of a JIT Compiler optimized for better overall performance.

C2 observes and analyzes the code over a longer period of time compared to C1. This allows C2 to make better optimizations in the compiled code.

It can be sued for long-running server-side applications.

# Tiered Compilation

The C2 compiler often takes more time and consumes more memory to compile the same methods. However, it generates better-optimized native code 
thant that produced by the C1.

Tiered compilation's goal is to use a mix of C1 and C2 compilers in order to achieve both fast startup and good long-term performance.

First, the JIT Compiler compiles the frequently executed sections of code with C1 to quickly reach native code performance. Later, C2 kicks in 
when more profiling information is available. C2 recompiles the code with more aggressive and time-consuming optimizations to boost performance.

In summary, C1 improves performance faster, while C2 makes better performance improvements based on more information about hotspots.


## Accurate Profiling

JVM collects profiling information on the C1 compiled code. \
Since the compiled code achieves better performance, it allows the JVM to collect more profiling samples.

## Code Cache

Code cache is a memory area where the JVM stores all bytecode compiled into native code. 

# Compilation Levels

Even though the JVM works with only one interpreter and two JIT compilers, there are **five possible levels of compilation.** The reason behind 
this is that the C1 compiler can operate on three different levels.

## Level 0 (Interpreter Code)

Initially, JVM interprets all Java code.

## Level 1 (Simple C1 Compiled Code)

The JVM compiles the code using the C1 compiler, but without collecting any profiling information. **The JVM uses level 1 for methods that are 
considered trivial.**

**Due to low method complexity(= trivial), the C2 compilation wouldn't make it faster.** \
Thus, the JVM concludes that there is no point in collecting profiling information for code that can not be optimized further.

## Level 2 (Limited C1 Compiled Code)

The JVM compiles the code using the C1 compiler with light profiling. **The JVM uses this level when the C2 queue is full.** The goal is to 
compile the code as soon as possible to improve performance.

**Later, the JVM recompiles the code on level 3 using full profiling. Finally, once the C2 queue is less busy, the JVM recompiles it one level 4.**

## Level 3 (Full C1 Compiled Code)

The JVM compiles the code using the C1 compiler with full profiling. **Thus, the JVM uses it in all cases except for trivial methods or when 
compiler queues are full.**

**The most common scenario in JIT compilation is that the interpreted code jumps directly from level 0 to level 3 (level 0 → level 3)**

## Level 4 (C2 Compiled Code)

The JVM compiles the code using the C2 compiler for maximum long-term performance. The JVM uses this level to compile all methods except trivial ones.

Given that level 4 code is considered fully optimized, the JVM stops collecting profiling information. 

However, it may decide to de-optimize the code and send it back to level 0.

# References

* https://www.baeldung.com/jvm-tiered-compilation