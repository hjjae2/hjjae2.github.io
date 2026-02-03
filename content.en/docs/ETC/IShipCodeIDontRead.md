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

## Reference

https://news.hada.io/topic?id=26222

## ✔️ AI 에이전트 기반 워크플로우의 10가지 핵심 교훈

* **완벽주의를 버리기**: 코드가 항상 자신의 취향에 맞지 않을 수도 있다는 사실을 받아들이면, 에이전트를 다룰 때 더욱 효율적으로 일할 수 있음
* **루프를 닫을 것**: AI 에이전트가 스스로 컴파일, 린트, 실행, 검증할 수 있도록 시스템 설계 필요
* Pull Request는 죽었고, **"Prompt Request"가 부상**: 코드 자체보다 코드를 생성한 프롬프트를 보는 것이 더 중요
* **코드 리뷰는 사라지고 아키텍처 논의가 대체**: Discord에서도 핵심 팀과 코드가 아닌 아키텍처와 큰 결정만 논의
* 5~10개 에이전트를 동시에 운영하며 "몰입(flow) 상태" 유지
    * 각 에이전트가 서로 다른 기능을 병렬로 작업
* **계획 수립에 상당한 시간 투자**, Codex 선호
    * 에이전트와 반복적으로 대화하며 견고한 계획 수립
    * 계획에 도전하고, 수정하고, 반박한 후 만족하면 실행하고 다음으로 이동
    * Codex는 장시간 작업을 독립적으로 수행하지만, Claude Code는 명확화를 위해 자주 돌아와서 산만해짐
* **의도적으로 덜 구체적인 프롬프트를 사용해 예상치 못한 솔루션을 발견하기도 함**
* 로컬 CI가 원격 CI보다 우수: 원격 CI의 10분 대기 시간 대신 에이전트가 로컬에서 테스트 실행
* **대부분의 코드는 지루한 데이터 변환**: 집착할 필요 없고, 에너지는 시스템 설계에 집중해야 함
* 구현 세부사항보다 **결과물에 관심을 가진 엔지니어가 AI와 잘 협업**
    * 알고리듬 퍼즐 풀기를 좋아하는 엔지니어는 "AI 네이티브" 전환에 어려움을 겪음
    * 제품 출시를 좋아하는 사람이 더 잘 적응

## 💡 그럼 코드 복잡도 리뷰는 이제 의미 없나?

#### ❌ 옛날 복잡도

* cyclomatic complexity
* nested depth
* Line of Code(LOC)

👉 AI가 다 해결함.

#### ✅ 미래 복잡도

👉 Operational Complexity

* 장애 전파 구조
* 데이터 정합성
* 배포 리스크
* 롤백 가능성
* migration 난이도
* lock-in

## ✅ 앞으로 강화해야 할 능력

⭐ 1. 시스템 설계 능력 (최우선)

> “이 구조 장애 나면 어떻게 터질까?”

⭐ 2. Distributed System 이해

* consistency
* partition tolerance
* idempotency
* backpressure

⭐ 3. Failure Engineering

> “어떻게 망가질까?”

⭐ 4. AI를 “코드 생성기”가 아니라 “가속기”로 쓰는 능력

❌ AI에게 맡김 <br/>
✅ AI를 5배 속도 도구로 사용
