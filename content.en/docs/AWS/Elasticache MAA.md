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

# Memory access amortization

Random memory access is an expensive operation. AWS introduced memory access amortization(MAA), **a technique to reduce memory access costs**, especially for big dynamic data structures such as those used in redis.

**MAA interleaves the steps from many data structure operations to ensure parallel memory access and reduces memory access latency**. In Elasticache for Redis 7.1 AWS applied this method on the redis dictionary, and we get up to 60% reduction in hash-find, and accelerated commands.

This is depicted in the following example, where the main Redis processing thread receives three parsed commands from the enhanced I/O threads. **Next, it interleaves a dictionary-find procedure to simultaneously prefetch needed data into the CPU cache.**

![](/images/ElasticacheMAA.png)

# References

https://aws.amazon.com/ko/blogs/database/achieve-over-500-million-requests-per-second-per-cluster-with-amazon-elasticache-for-redis-7-1/