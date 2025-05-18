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

## Overview
In JVM, memory area has two regions. One of them is called **‘Stack’** and the other one is **’Heap’**.

## What is Stack?
This area is not big enouch to store objects so **what is happening is (1) primitive types and (2) object pointers can be stored.**

## What is Heap?
Heap size is bigger than stack because heap is **the main region for holding objects.** Every created object is held in heap, and its reference is helding in stack.

Heap has 4 different areas/generations(Eden, S0, S1, Old) on 2 main generations(Young, Old).
* Young(Eden, S0, S1)
* Old

Let’s see what they do.
1. Created objects are first placed in **‘eden’** space.
2. Eden is fulled, objects are moved to the '**S0**' or '**S1**'.
3. And then repeat 1, 2.
    1. Created objects are placed in eden again.
    2. When eden is full, both eden and S0, S1 will be moved to the S0, S1
4. If objects are moved more than five, those objects are now placed in ‘**old**' generation.
    1. If there are no variables in the stack that holds its reference, this means that objects is eligible for GC.

### What is Metaspace?
Besides these regions, there is one more region in memory. Metaspace is the region where application’s metadata is stored.
This space has many missions. **One of them is holding static variables, methods, classes.**

## References
[How Java Memory Works?](https://medium.com/stackademic/how-java-memory-works-c751460e3cbd)

