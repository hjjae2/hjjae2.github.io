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

## Glossary
| Term                  | Definition                                                   |
|-----------------------|--------------------------------------------------------------|
| Evacuation            | 객체의 Copy, Moving 을 의미                                        |
| Collection Set (CSet) | GC 수행될 Region 집합<br>GC 이후 CSet 에 포함된 대상 Region 은 모두 비워짐(복사 혹은 이동)<br>CSet 이 JVM 에서 차지하는 비율은 1% 이내 |
| Remembered Set (RSet) | Reference 를 가진 객체가 어느 Region 에 위치하는지 기억하기 위해 사용하는 자료구조<br>Region 당 하나의 RSet 이 존재 -> Region 의 병렬 및 독립된 처리를 가능하게 함<br>RSet 이 차지하는 비율(= Region 내?) 5% 이내 |
| Mixed Collection      | Young, Old Generation 에서 GC 가 일어나는 것을 의미<br>Mixed GC, Mixed Collection 이라고도 부름 |
| Humongous Region      | Region 크기의 50%를 초과하는 큰 객체를 저장하기 위한 공간<br>이 Region 에서는 GC 가 최적으로 동작하지 않음 |

## Region
이전의 GC 에서는 힙 영역을 Young, Old 논리/물리적으로 구분지어 관리했다. G1GC 에서는 논리적으로 Young, Old 영역이 존재하긴 하지만, 물리적으로는 Region 이라는 개념을 사용해 관리한다. 이를 테면, 힙 영역을 동일 크기의 Region 으로 나눠 관리한다. **G1GC 에서는 각 Region 에서 살아있는 객체가 가장 적은(= 정리 대상이 가장 많은) Region을 판단하고 정리하는 것이 목표다. ⭐**
> 이때, GC 가 수행될 Region 목록을 갖고 있는 자료구조가 ‘CollectionSet(CSet)’ 이다.

각 Region 은 Eden, Survivor, Old, Humongous 등의 역할을 수행할 수 있고, 필요에 따라 동적으로 할당된다.\
각 Region 은 독립적으로 가비지 컬렉션을 수행할 수 있다. **즉, 병렬적으로 수행이 필요한 Region 만 가비지 컬렉션을 수행할 수 있다는 의미이다.** \
각 Region 의 크기는 ‘힙 크기 / (Region)개수’로 결정된다.
* Region 의 개수는 기본 값 ‘2048’ 이다.
* `-XX:G1HeapRegionSize` 설정으로 수정할 수 있다.

## How to work?
다음 단계로 가비지 컬렉션을 수행한다.

1. **Initial Mark** : GC 루트에서 직접 참조되는 객체를 마킹한다.
   이 단계는 일반적으로 Young GC 와 함께 수행되어 오버헤드를 줄인다.
> **⚠️ GC 루트란?** \
> 살아있는 객체를 식별하기 위한 최초 시작점을 의미하는 것으로 보인다. 예를 들어, 힙 영역의 객체를 참조하고 있는 스택 영역의 참조 변수(스레드, 메서드 내 지역 변수 등)가 있겠다. 정적 변수와 JNI 참조도 있다는데, 이에 대해선 다시 한번 살펴보자. ✔︎

> **⚠️ G1GC 에서 말하는 Young GC 란?** \
> G1GC 에서의 Young 영역은 Eden, Survivor 영역이다. 이 영역들의 객체를 정리하는 과정을 의미한다. 이를 테면, (1) Eden 영역에 새로 생성된 객체들 (2) 이전 GC 에서 Survivor 영역으로 이동한 객체들이 대상이 되겠다.
> 살아있는 객체들은 Survivor 영역이나 Old 영역으로 이동되고 그렇지 않은 객체들은 정리된다.

2. **Root Region Scan** : Survivor 영역의 루트 객체를 스캔해, 살아있는 객체를 식별한다.
> **⚠️ Survivor 영역의 루트 객체란?** \
> (1) GC 루트로부터 직접 참조되거나 (2)이전 GC 사이클에서 살아남아 Survivor 영역에 존재하고 있는 객체들을 의미한다.

3. **Concurrent Mark** : 힙 전체를 스캔해, 살아있는 객체를 식별한다.
   이 단계는 애플리케이션 스레드와 동시에 수행된다.

4. **Remark** : Concurrent Mark 단계에서 변경된 객체를 다시 식별해 정확성을 확보한다.
   이 단계는 STW 가 발생한다.

5. **Cleanup** : 가비지가 많은 Region 을 식별하고 필요에 따라 Region 을 회수한다.

6. **Copy & Evacuation** : 살아있는 객체를 새로운 Region 으로 복사해 메모리 단편화를 줄이고, 회수된 Region 은 재사용 가능한 상태로 만든다.
   G1GC 는 힙 내의 하나 이상의 Region 을(= 객체를) 단일 Region 으로 복사하는데, 이때 메모리를 압축하거나 해제한다고 한다.

## Young GC in G1GC?
Eden, Survivor 영역 대상으로 진행된다. **GC 루트에서 도달 가능한 객체만 생존한다.** 생존한 객체들은 (1)Survivor 영역 (2)Old 영역으로 이동된다. G1GC 는 병렬 처리를 통해 다수의 Region 에 대한 정리를 동시에 처리할 수 있다. 추가로, Young GC 는 STW 를 유발한다.
> **⚠️ -XX: MaxTenuringThreshold ?** \
> GC 에서 살아남을 때마다 나이가 증가하는데, MaxTenuringThreshold 값에 도달하면 Old 영역으로 승격된다. 기본 값은 ’15’ 이다.

##### 실행 조건
1. **IHOP(Initiating Heap Occupancy Percent) 에 의해** \
   Young 영역이 꽉 차진 않았지만, Old 영역이 거의 다 차고 있는 상황(= IHOP 임계치를 넘어선 상황)에 Young GC 가 발생할 수 있다. Concurrent Mark 단계가 시작되는데 이 단계의 초기 마킹 단계(= Initial Mark 단계를 의미하는건가? 🤔)에서 Young GC 가 실행될 수 있다고 한다.
> ⚠️ [Garbage First Garbage Collector Tuning](https://www.oracle.com/technical-resources/articles/java/g1gc.html) 에 설명되어 있는지 확인해보자.

`-XX:InitiatingHeapOccupancyPercent` 설정으로 값을 조절할 수 있다. 기본 값은 ’45(%)’이다.
> **⚠️ `InitiatingHeapOccupancyPercent` 이란?** \
> 힙 영역의 사용량 혹은 전체 힙 영역에서 Old 영역에 살아있는 객체의 비율(= Old 영역에 살아있는 객체 크기 / 전체 힙 영역 크기)을 의미한다고 한다. Old 영역에 가득 차기 전에 여유 공간을 만들어 Full GC 를 방지하는 것이 목적이라고 한다.
>
> [Simple & effective Java G1 GC tuning tips](https://blog.gceasy.io/simple-effective-g1-gc-tuning-tips/) 글을 참고할 수 있다.
> *“ Force G1 to start marking phase earlier. This can be achieved by lowering ‘-XX:InitiatingHeapOccupancyPercent’ value. Default value is 45. It means the **G1 GC** marking phase will begin only when heap usage reaches 45%. By lowering the value, the G1 GC marking phase will get triggered earlier so that **Full GC** can be avoided. “*

2. **Pause Time Goal 준수를 위해** \
   일시 정지 목표 시간(Pause Time Goal)을 준수하기 위해 미리 발생시킬 수 있다. 이 값을 엄격하게 준수(보장)하는 것은 아니고 노력하기 위한 목표 값 정도라고 한다.
   (1) GC 가 발생한 이후 정지된 시간을 측정하고
   (2) 목표한 시간보다 더 길게 동작했다면 Young 영역의 크기를 조정해 목표한 시간을 준수할 수 있게 한다.
   (3) 목표한 시간보다 더 짧게 동작했다면 이와 반대로 동작할테고
   (4) 결국 목표한 시간에 근접하게 Young 영역의 크기를 조절하게 될 것이다.

`-XX:MaxGCPauseMillis` 설정으로 값을 조절할 수 있다. 기본 값은 ‘200(ms)’ 이다.
> **⚠️ Pause Time Goal**
> [Garbage First Garbage Collector Tuning](https://www.oracle.com/technical-resources/articles/java/g1gc.html?utm_source=chatgpt.com) 글을 참고할 수 있다.
> *“The G1 GC uses concurrent and parallel phases to achieve its target pause time and to maintain good throughput. … The G1 GC has a pause time-target that it tries to meet (soft real time).“*
> [Part 1: Introduction to the G1 Garbage Collector](https://www.redhat.com/en/blog/part-1-introduction-g1-garbage-collector) 글을 참고할 수 있다.
> *“At its heart, the goal of the G1 collector is to achieve a predictable soft-target pause time, defined through -XX:MaxGCPauseMillis, while also maintaining consistent application throughput.”*

> ⭐ G1GC 는 Young 영역의 크기를 동적으로 조절한다. 해서, `-XX:NewRatio` 와 같은 Young 영역의 크기를 설정하는 것은 강력히 권장되지 않는다고 한다.

3. Young 영역이 가득 찼을 때 (= Young 영역 할당이 실패할 때) \
   객체를 Eden 영역에 할당하려고 하는데 실패할 때(Allocation Failure), Evacuation 이 실패할 때(To-Space Exhausted, Evacuation Failure) 발생할 수 있다.
   전자의 경우 일반적인 상황에서 발생할 수 있는 정도라고 한다.
   후자의 경우엔 높은 할당률, 높은 객체 생존률, 힙 단편화 등으로 인해 발생하며 심각한 메모리 압박 상황을 의미한다고 한다. GC 비용이 크고 STW 를 상당히 연장시킬 수 있다고 한다. 할당률을 조절하지 못하면 Full GC 가 폴백 메커니즘으로 동작할 수 있다고 한다. 이 실패를 방지하기 위해 G1 은 기본적으로 전체 힙 크기의 10% 를 여유 공간으로 유지한다고 한다. 이 공간은 Evacuation 을 위해 항상 사용 가능하도록 보장된다. 이 매개변수를 늘리면 GC 가 일찍 트리거되도록 강제해 이 실패를 완화하는데 더 도움이 될 수 있다고 한다. 🤔

`-XX:G1ReservePercent` 값을 조절해 여유 공간을 조절할 수 있다. 기본 값은 ’10(%)’ 이다.
> 이 내용은 좀 더 살펴봐야겠다. 🤔 \
> [Part 1: Introduction to the G1 Garbage Collector](https://www.redhat.com/en/blog/part-1-introduction-g1-garbage-collector)
> [HotSpot Virtual Machine Garbage Collection Tuning Guide](https://docs.oracle.com/en/java/javase/24/gctuning/garbage-first-garbage-collector-tuning.html)
> https://jira.atlassian.com/browse/CONFSERVER-43493

## References
* https://blog.leaphop.co.kr/blogs/42/G1GC_Garbage_Collector%EC%97%90_%EB%8C%80%ED%95%B4_%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0___1
* [Garbage First Garbage Collector Tuning](https://www.oracle.com/technical-resources/articles/java/g1gc.html?utm_source=chatgpt.com)
* [G1GC Fundamentals: Lessons from Taming Garbage Collection](https://product.hubspot.com/blog/g1gc-fundamentals-lessons-from-taming-garbage-collection?utm_source=chatgpt.com) (읽어보자.)

#notes/java