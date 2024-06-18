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

# DynamoDB?

KV 스토어, NoSQL 데이터베이스. 규모에 상관없이 10ms 미만의 성능을 제공한다.

KV 스토어이기에 Key 설계가 가장 중요하다. 이를 테면, 어떤 속성을 키(파티션 키, 정렬 키)로 설정할 지이다. 조회 시 Key 와 관련 없는 속성으로 조회하면 O(n) 이다. 요약하면, **(1) 접근 패턴에 따른 키 설계 (2) 분산 배치될 수 있는 키 설계**가 중요하다. 서버, 스토리지에 대해선 관리해준다. 관리자는 키 설계에 집중한다.

최대 레코드 크기는 400KB 이다. 이를 넘으면 S3 에 저장하고 DynamoDB 에는 메타데이터만 저장한다.

# 파티션 키

키에 대해 해싱 후 저장 위치(파티션)을 결정한다. **데이터가 분산되도록 설계하는 것이 중요하다.**

파티션 키는 변경할 수 없다. 변경을 원한다면, 새로운 테이블을 생성 후 데이터를 이동시키는 방법을 사용한다.

# REST API

REST API 기반의 Public API(AWS Service Endpoint) 를 통해 조작 가능하다. 

AWS Service Endpoint 에 대해선 https://docs.aws.amazon.com/general/latest/gr/rande.html 문서를 참고할 수 있다.

# 보조 인덱스 (Secondary Index)

## LSI (Local Secondary Index)

* 새로운 공간(스토리지)에 데이터를 복제하고 LSI 기준으로 정리해 갖고 있는 구조이다.
* 파티션 키는 동일하고 정렬 키만 다르게 설정할 수 있다.
* 테이블 생성 시 결정해야 한다. 테이블 생성 후 변경할 수 없다.
* **강력 일관성, 최종 일관성이 허용된다.** 

## GSI (Global Secondary Index)

* 새로운 테이블을 만드는 것과 같다. 가능한 적게 만들어야 비용이 적다.
  * 기존 테이블의 내용을 새로운 테이블에 복제한다.
  * 이때 기존 테이블의 항목과 동일할 필요는 없다.
* 파티션 키와 정렬 키를 임의로 설정할 수 있다.
* **최종 일관성만 허용된다.**

# RCU, WCU

## RCU (Read Capacity Unit)

1 RCU 는 다음을 의미한다.

* 최대 4KB 항목 1개에 대해 초당 1건의 강력한 일관성 읽기 요청을 처리할 수 있다. 또는 초당 2건의 최종 일관성 읽기 요청을 처리할 수 있다.
* 항목 크기, 일관성에 따라 필요한 단위가 결정된다.
  * 강력한 일관성 : 1
  * 최종 일관성 : 0.5
  * 트랜잭션 읽기 : 2
* 4KB 를 넘는 항목은 여러 RCU 가 필요하다.

## WCU (Write Capacity Unit)

1 WCU 는 다음을 의미한다.

* 최대 1KB 항목 1개에 대해 초당 1건의 쓰기 요청을 처리할 수 있다.
* 1KB 를 넘는 항목은 여러 WCU 가 필요하다.
* 트랜잭션 쓰기는 최대 1KB 항목에 대해 2 WCU 가 필요하다.

# Streams

변경 사항을 기록한다. 스트림 처리를 위해 추가 공간(스토리지)가 필요하다.

샤드 형태로 저장된다.

# Locking

Lock 개념/기능 없다. 클라이언트에서 처리해야 한다.

# 결론

키 설계에 집중하자. (1) 접근 패턴에 따른 키 설계 (2) 분산 배치될 수 있는 키 설계 를 기억하자.