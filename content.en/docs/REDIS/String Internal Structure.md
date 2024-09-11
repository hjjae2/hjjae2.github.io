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


# 요약

값(value)은 클라이언트에게는 문자열로 전달받지만 내부적으로는 정수와 문자열로 구분해 관리한다. 정수는 공유 정수(0~9999)와 독립 정수로, 문자열은 embedded string과 raw로 구분된다. 이렇게 4가지로 구분해 관리하는 이유는 **(1)
메모리 절약, (2) 성능 향상**을 위해서라고 한다.

문자열 저장 시 두 개의 메모리 영역이 사용된다. 

**1. redisObject**
외부 데이터 타입(string, set, hash, ...), 내부 데이터 타입(int, embstr, raw), 실제 값을 가리키는 포인터(*ptr), 참조 횟수(refCount), lru 정보(lru) 등을 저장한다.

**2. sdshdr**
실제 문자열 데이터를 저장한다. 문자열 길이(len), 문자열 뒤의 여유 공간(free), 문자열 데이터(buf) 등을 저장한다.

## Embedded String

문자열이 44바이트 이하일(3.2 이전 버전은 39바이트) 경우 embedded string으로 저장된다. 위 소개한 redisObject, sdshdr 가 개별적인 메모리 영역으로 관리되는 것이 아니고 하나의 메모리 영역으로 관리된다. 이로 인해 메모리 절약과 성능 향상이 가능하다. 44바이트(3.2 이전 버전은 39바이트) 이상의 문자열은 raw string으로 저장된다.

> 처리 시 메모리 값을 CPU 캐시로 가져와야 하는데, 메모리 영역이 하나로 관리되면 CPU 캐시에 한 번에 올라가기 때문에 성능 향상이 가능하다.

44 바이트가 기준인 이유는 jemalloc() 의 메모리 할당 단위와 관련이 있다. jemalloc 은 요청한 만큼 할당하지 않고 32, 64, 128 과 같이 사전에 정해둔 단위로 할당한다. 이렇게 할당하는 이유는 단편화 문제를 개선하기 위함이고. 
이때 redisObject 와 sdshdr 를 할당하기 위해 최소 필요한 바이트가 redisObject 16바이트, sdshdr 3바이트, null term 1바이트이므로 총 25바이트가 필요하다. 이를 64바이트로 맞추기 위해 문자열 길이가 44바이트 기준이 된다.

# References

* http://www.redisgate.kr/redis/configuration/internal_string.php
