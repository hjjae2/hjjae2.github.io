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

## 개요

레디스에 해시 값(64byte)의 존재 여부를 확인하는 로직을 구현하면서, 레디스의 메모리 사용량에 대해 궁금해졌습니다.

저장하려는 데이터의 형태는 다음과 같습니다.

`fffffe0f3bc236317703e574d41c13fc43c6954d7dfb2ae7ce1845488761e5cf`

자료 구조에 따라, 저장 형태에 따라 레디스의 메모리 사용량이 달라질 수 있습니다.

이번 글에서는 자료구조에 따른 메모리 사용량을 비교, 확인해봅니다.

단순 String 형태로 저장하는 것과 Set, Hash, HyperLogLog 형태로 저장하는 것을 비교할 것입니다.

자료구조 별로 약 100만개의 데이터를 저장하고, 메모리 사용량을 비교해보겠습니다.

## String (with TTL)

| 분류                                                                                | 값                                                                                |
|-----------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| 명령어                                                                               | `SET fffffe0f3bc236317703e574d41c13fc43c6954d7dfb2ae7ce1845488761e5cf 1 EX 3600` |
| keys.count	                                                                       | 998,686                                                                          |
| total.allocated                                                                   | 248,631,960 (237MiB)                                                             |
| overhead.total <br/> - overhead.hashtable.main <br/> - overhead.hashtable.expires | 82,226,198 (78MiB) <br/> - 48,336,048 (46.1MiB) <br/> - 32,357,072 (30.9MiB)     |

## String (without TTL)

| 분류                                             | 값                                                                        |
|------------------------------------------------|--------------------------------------------------------------------------|
| 명령어                                            | `SET fffffe0f3bc236317703e574d41c13fc43c6954d7dfb2ae7ce1845488761e5cf 1` |
| keys.count	                                    | 998,686                                                                  |
| total.allocated                                | 216,274,888 (206MiB)                                                     |
| overhead.total <br/> - overhead.hashtable.main | 49,868,102 (48MiB) <br/> - 48,336,048 (46.1MiB)  <br/> - 0               |

## Set

| 분류                                                                                | 값                                                                                                                                                                                       |
|-----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 명령어                                                                               | `SADD test:1:500000 fffffe0f3bc236317703e574d41c13fc43c6954d7dfb2ae7ce1845488761e5cf` <br/> `SADD test:500001:1000000 fffffe0f3bc236317703e574d41c13fc43c6954d7dfb2ae7ce1845488761e5cf` |
| keys.count	                                                                       | 2 (키 당 요소 50만개)                                                                                                                                                                            |
| total.allocated                                                                   | 113,959,376 (108 MiB)                                                                                                                                                                   |
| overhead.total <br/> - overhead.hashtable.main <br/> - overhead.hashtable.expires | 1,532,166 (1.5MiB) <br/> - 112 <br/> - 0                                                                                                                                                |

## Hash

| 분류                                                                                | 값                                                                                                                                                                                           |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 명령어                                                                               | `HSET test:1:500000 fffffe0f3bc236317703e574d41c13fc43c6954d7dfb2ae7ce1845488761e5cf 1` <br/> `HSET test:500001:1000000 fffffe0f3bc236317703e574d41c13fc43c6954d7dfb2ae7ce1845488761e5cf 1` |
| keys.count	                                                                       | 2 (키 당 요소 50만개)                                                                                                                                                                                |
| total.allocated                                                                   | 121,959,376 (116 MiB)                                                                                                                                                                       |
| overhead.total <br/> - overhead.hashtable.main <br/> - overhead.hashtable.expires | 1,532,166 (1.5MiB) <br/> - 112 <br/> - 0                                                                                                                                                    |

## Hash (using ziplist)

| 분류                                                                                | 값                                                                                                                                                                                    |
|-----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 명령어                                                                               | `HSET test:1:1000 fffffe0f3bc236317703e574d41c13fc43c6954d7dfb2ae7ce1845488761e5cf 1` <br/> `HSET test:1001:2000 fffffe0f3bc236317703e574d41c13fc43c6954d7dfb2ae7ce1845488761e5cf 1` |
| keys.count	                                                                       | 1031 (키 당 요소 1000개)                                                                                                                                                                  |
| total.allocated                                                                   | 86,166,984 (82 MiB)                                                                                                                                                                  |
| overhead.total <br/> - overhead.hashtable.main <br/> - overhead.hashtable.expires | 1,590,702 (1.5MiB) <br/> - 57,624 <br/> - 0                                                                                                                                          |

## HyperLogLog

| 분류                                                                                | 값                                                                                                                                                                                        |
|-----------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 명령어                                                                               | `PFADD test:1:500000 fffffe0f3bc236317703e574d41c13fc43c6954d7dfb2ae7ce1845488761e5cf` <br/> `PFADD test:500001:1000000 fffffe0f3bc236317703e574d41c13fc43c6954d7dfb2ae7ce1845488761e5cf` |
| keys.count	                                                                       | 2 (키 당 요소 50만개)                                                                                                                                                                          |
| total.allocated                                                                   | 1,599,248 (1.53MiB)                                                                                                                                                                      |
| overhead.total <br/> - overhead.hashtable.main <br/> - overhead.hashtable.expires | 1,532,166 (1.5MiB) <br/> - 112 <br/> - 0                                                                                                                                                 |

## 결론

레디스는 다양한 자료구조를 지원합니다. 각 자료구조는 다양한 용도에 맞게 사용됩니다. 이번 글에서는 각 자료구조의 메모리 사용량을 비교해보았습니다.

**결과를 통해 메모리 오버헤드는 키 개수와 TTL 개수에 따라 달라짐을 확인할 수 있었습니다.** 즉, 키 개수와 TTL 개수에 따라 해시 테이블 사이즈가 결정되며, TTL 관리를 위한 해시 테이블이
생성/관리된다는 것을 알 수 있었습니다.

또, 특정 사이즈(entry, value)까지의 데이터는 더 효율적인 자료구조 형태로 인코딩된다는 것을 확인할 수 있었습니다. 이에 대한 자세한
내용은 [using-hashes-to-abstract-a-very-memory-efficient-plain-key-value-store-on-top-of-redis](https://redis.io/docs/management/optimization/memory-optimization/#using-hashes-to-abstract-a-very-memory-efficient-plain-key-value-store-on-top-of-redis)
를 참고하시기 바랍니다.

값의 존재 여부를 확인하기 위한 자료구조로 Bloom Filter 사용을 고려해볼 수 있습니다. Bloom Filter 는 확률적 데이터 구조로, 적은 메모리로 많은 데이터를 저장하고, 존재 여부를 확인할 수
있습니다. 이에 대한 자세한 내용은 [Bloom Filter](https://redis.io/docs/data-types/probabilistic/bloom-filter/)를 참고하시기 바랍니다.
