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

# Memory Optimization

## Special encoding of small aggregate data types

Since Redis 2.2, many types are optimized to use less space up to a certain size.

Hashes, Lists, Sets composed of just integers, and Sorted Sets, when smaller than a given number of elements, and up to a maximum element size, are encodded in a very memory-efficient way that uses up to 10 times less memory (with 5 times less memory used being the average saving).

Since this is a CPU / Memory tradeoff it is possible to tune the maximum number of elements and maximum element size for special encodded types using the following redis.conf directives (defaults are shown)

**Redis <= 6.2**
```sh
hash-max-ziplist-entries 512
hash-max-ziplist-value 64
zset-max-ziplist-entries 128
zset-max-ziplist-value 64
set-max-intset-entries 512
```

**Redis >= 7.0**
```sh
hash-max-listpack-entries 512
hash-max-listpack-value 64
zset-max-listpack-entries 128
zset-max-listpack-value 64
set-max-intset-entries 512
```

**Redis >= 7.2**

The following directives are also available

```sh
set-max-listpack-entries 128
set-max-listpack-value 64
```

**If a specially encoded value overflows the configured max size, Redis will automatically convert it into normal encoding.**

## Using 32-bit instances

When Redis is compiled as a 32-bit target, it uses a lot less memory per key, since pointers are small, but such an instance will be limited to 4GB of maximum memory usage.

RDB and AOF files are compatible between 32-bit and 64-bit instances so you can switch from 32 to 64, or the contrary, without problems.

## Bit and Byte level operations

There are bit and byte level operations: GETRANGE, SETRANGE, GETBIT, SETBIT.

Using these commands you can treat the Redisd string type as a random access array.

*(...skip)*

## Use hashes when possible

## Using hashes to abstract a very memory-efficient plain key-value store on top of Redis

Basically it is possible to model a plain key-value store using Redis where values can just be strings, which is not just more memory efficient than Redis plain keys but also much more memory efficient than memchaced.

> What is key-value model in this sentence? :thinking:

Let's start with some facts: a few keys use a lot more memory than a single key containing a hash with a few fields. How is this possible? We use a trick. **In theory to gurantee that we perform lookups in constant time (also known as O(1)), there is the need to use a data structure like a hash table.**

Many times hashes contain just a few fields. When hashes are small, we can instead just encode them in an O(N) data structure, like a linear array with length-prefixed key-value pairs.
> Since we do this only when N is small, the amortized time for HGET and HSET commands is still O(1)

**The hash will be converted into a real hash table as soon as the number of elements it contains grows too large (you can configure the limit in redis.conf)** :star:

However since hash fields and values are not (always) represented as full-featured Redis objects, hash fields can't have an associated TTL, and can only contain a string.
> But we are okay with this, this was the intention. We trust simplicity more than features, so nested data structures are not allowed, as expires of single fields are not allowed.

So hashes are memory efficient. This is useful when using hashes to represent objects or to model other problems when there are group of related fields.

### But what about if we have a plain key value business? 

Imagine we want to use Redis as a cache for many small objects, which can be JSON encodded objects, small HTML fragments, simple key -> boolean values and so forth. Basically, anything is a string -> string map with small keys and values.

Now let's assume the objects we want to cache are numbered, like:
* object:102393
* object:1234
* object:5

So we use all the characters but the last two for the key, and the final two characters for the hash field name. To set our key we use the following command:

```sh
HSET object:12 34 "my-value"
HSET object:1023 93 "my-value"
```

As you can see every hash will end up containing 100 fields, which is an optimal compromise between CPU and memory saved.

And see this instagram article for example : [Storing hundreds of millions of simple key-value paris in Redis](https://instagram-engineering.com/storing-hundreds-of-millions-of-simple-key-value-pairs-in-redis-1091ae80f74c)

**How much memory do we save this way?**

This is the result against a 64-bit instance of Redis 2.2 (w/ 100,000 item):
* USE_OPTIMIZATION set to true: 1.7 MB of used memory
* USE_OPTIMIZATION set to false; 11 MB of used memory

> *WARNING: for this to work, make sure that in your redis.conf you have something like this:*
>
> * `hash-max-zipmap-entries 256`
> * `hash-max-zipmap-value 1024`

**Every time a hash exceeds the number of elements or element size specified it will be converted into a real hash table, and the memory saving will be lost.**

### Why don't you do this implicitly in the normal key space so that users don't have to care?

There are two reasons:

1. we tend to make tradeoffs explicit, and this is a clear tradeoff between many things: CPU, memory, and max element size.
2. Top-level key space must support a lot of interseting things like expires(TTL), LRU data, and so forth so it is not practical to do this in a general way.

## Memory allocation

To store user keys, Redis allocates at most as much memory as the `maxmemory` setting enables.

> However, there are small extra allocations possible.

*(...skip)*

# Reference 

https://redis.io/docs/management/optimization/memory-optimization/
