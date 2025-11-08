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

## 배경

레디스에 저장된 데이터를 덮어쓰는 과정에서 간헐적으로 조회 실패 현상이 발생한다. \
SpringDataRedis의 `RedisKeyValueAdapter.put` 메서드를 통해 데이터 쓰기를 시도했는데, 내부에서 처리될 때 호출되는 `GET` 요청이 간헐적으로 실패했다.

## 원인

![img.png](/images/springdataredis.png)

저장(save) 로직 중 아이템 삭제 후 다시 삽입하는 로직이 존재한다. 
즉, 저장 로직과 조회 로직이 동시에 실행되면서 발생할 수 있다.

## 조치

partialUpdate (question)  (https://github.com/spring-projects/spring-data-redis/issues/1826#issuecomment-752635904)

## 참고

3.2.0 버전도 동일 (https://github.com/spring-projects/spring-data-redis/blob/05ddd846df5645c56d70757ab2242034ab92afa9/src/main/java/org/springframework/data/redis/core/RedisKeyValueAdapter.java#L217)
