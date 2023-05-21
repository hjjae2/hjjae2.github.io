---
type: 'docs'
title: "컨슈머 랙(Consumer Lag)"
bookCollapseSection: false
bookHidden: false
bookToC: true
bookComments: false
bookSearchExclude: false
bookFlatSection: true
weight: 1
---

> 파티션에 데이터가 들어갈 때 offset 이라는 숫자가 붙게 된다. (0 부터 시작한다.)

- 컨슈머의 상태를 추측할 수 있음 (모니터링 지표)
  - 각 파티션의 offset 기준으로, 컨슈머의 상태를 확인할 수 있도록 함

- **lag은 프로듀서 생산 속도(offset), 컨슈머 소비 속도(offset)의 차이를 기반으로 함**
  - lag은 여러개가 될 수 있음 (파티션별로)
  - `records-lag-max` : lag 값 중 가장 큰 값

**컨슈머가 느리거나, 정상적으로 동작하지 않으면 lag 이 필연적으로 발생한다고 한다.**

<br><br>

### 모니터링 애플리케이션, 카프카 버로우(Burrow)

> 컨슈머 쪽에서 ELK, Grafana 쪽으로 데이터를 보내 확인할 수도 있으나, 컨슈머에 의존하게 되는 문제가 있다. 컨슈머가 비정상적으로 종료되거나, 모든 컨슈머에 이런 처리를 해줘야되는 문제가 있음

컨슈머 lag 모니터링을 도와주는 독립적인(외부) 애플리케이션

> 사용하는 것을 강력 권장한다. (사용하지 않을 이유가 없다고 한다.)

- Multi kafka cluster 지원 (Kafka 클러스터 여러 개를 지원)
- Sliding window 를 통한 컨슈머의 status(error, warning, ok) 확인할 수 있다. (?)
- HTTP API 제공한다.