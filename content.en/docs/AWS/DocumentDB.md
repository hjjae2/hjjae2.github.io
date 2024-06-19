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

# DocumentDB?

MongoDB **호환성**을 가진 AWS 서비스다. '호환성'에 주의하자, 완전히 동일한 것은 아니다. 문서를 참고해야 한다.

전반적으로 Aurora 와 비슷한 내부 구조(컴퓨팅 영역 ↔ 스토리지 영역, 쿼럼 등)로 구성되어 있다.

# vs DynamoDB

* DynamoDB 는 KV 스토어, NoSQL 데이터베이스이다. 
* DynamoDB 는 사용한 만큼 비용을 지불한다.
  * DocumentDB 는 프로비저닝된 내용에 대해 비용을 지불한다.
* DynamoDB 는 PB 까지 동적 확장 가능하다.
  * DocumentDB 는 128TB 까지 확장 가능하다. 128TB 이상 사용하려면 샤딩 기법을 이용해야 한다.
  * 따라서, 용량에 대해 고려해야 한다.

# vs RDS

* RDS 는 스키마가 정의되어 있다.
  * DocumentDB 는 스키마가 유연하다. 다만, 중복 데이터가 발생한다. 예를 들어, 'id' 필드를 저장하는 경우 RDS 는 1, 2, 3, ... 으로 저장하지만 DocumentDB 는 id: 1, id: 2, id: 3, ... 으로 저장한다.

# 구조

Aurora 와 동일하게 컴퓨팅, 스토리지 영역으로 나뉜다. 스토리지 영역은 셀 단위로 구성된다. 복제 개수(6개)도 동일하며, 쿼럼 구성(개수)도 동일하다.

읽기 전용 복제본 생성 시 EC2 만 생성하고 스토리지는 공유한다.

# 일관성

Primary 에 읽기 요청을 보내면 '강력한 일관성'이겠다. DocumentDB는 엔드포인트가 여러 개 있을 수 있다. 따라서 의도적으로 Primary 엔드포인트에 읽기 요청을 보낼 수 있다.

# 설계

* 8KB 이하 문서에 적합하다. 그 이상의 문서는 대용량 문서이며 I/O가 더 많이 발생한다.
* 문서 간 조인 없이 검색 가능하도록 설계한다.
  * 단순하게 구성한다. 이를 위해 데이터 비정규화를 수행할 수 있다. 또, 중첩 문서(임베딩된 문서) 형태를 활용하는 것이 낫다.
* 문서 크기, 비정규화, 원자성, 접근 패턴을 고려한다.
* 컬렉션 사이즈는 가능한 작게 유지하는 것이 좋을 수 있다. 상황에 따라 다를 수 있긴 하겠다.

# ReadPreference

* primaryPreferred: Primary 에 읽기 요청을 보낸다.
* secondaryPreferred: Secondary(복제본) 에 읽기 요청을 보낸다.
* nearPreferred: 가장 가까운 엔드포인트에 읽기 요청을 보낸다.

primaryPreferred, secondaryPreferred 는 일관성과 관련이 있겠다.

# Streams

컬렉션이 변경되면 기록한다. 추가 IOPS, 스토리지 비용이 발생한다.

DynamoDB Streams 와 비슷하다.

# 인덱스

* ESR(Equity Sort Range) 규칙
  *     * https://www.mongodb.com/docs/manual/tutorial/equality-sort-range-rule/
* 미사용 인덱스는 가능하면 삭제한다.
  * 다른 스토리지도 마찬가지겠지만 인덱스 추가 시 쓰기 성능이 저하될 수 있다. 즉, 스토리지, 메모리 IOPS 가 증가할 수 있다. 이 점을 고려해 가능한 필요한 인덱스만 설정하는 것이 좋다.

