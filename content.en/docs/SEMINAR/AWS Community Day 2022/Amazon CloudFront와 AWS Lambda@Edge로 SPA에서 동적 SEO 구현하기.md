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

## OpenGraph 

<br>

## SPA 는 왜 링크마다 OpenGraph 적용이 안될까?

OpenGraph 스크랩 봇의 행동 강령 : JS는 버린다. (JS 를 읽는데에 시간이 많이 쓰이기 때문에)
- 이건 별도로 찾아보자.

SPA 는 기본 뼈대가 아주 단순한 구조이다.

<br>

### 대처 방법은?

SSR, Static 웹으로 제공할 수 있다.

다만 위 방법도 단점은 있다. 동적으로 제공하기 힘들다.

<br>

### 코드 레벨이 아닌 인프라로 해결할 수 있을까?

인프라로 해결할 수 있다. **웹 앱 앞에 프록시를 두고 메타 태그를 추가할 수 있다.**

Lambda@Edge 를 통해 구현할 수 있다.

![](/images/[서버리스]%20Amazon%20CloudFront와%20AWS%20Lambda@Edge로%20SPA에서%20동적%20SEO%20구현하기_15.png)

<br><br>

## Lambda@Edge

CF 기능 중 하나로 사용자와 가장 가까운 위치의 람다를 실행하게 하는 서비스이다.

![](/images/[서버리스]%20Amazon%20CloudFront와%20AWS%20Lambda@Edge로%20SPA에서%20동적%20SEO%20구현하기_19.png)

<br>

### CF 캐싱 규칙 (캐싱 정규화)

> 어떤 캐싱 기준(URL, Request Object, ...)을 설정할 것인지?에 대한 내용이다. AWS 에서 기본적으로 제공하는 기준을 사용할 수도 있고 커스텀하게 설정하여 사용할 수도 있다.

1. Bot / User 여부
2. Bot 이라면, 해당 (단건)페이지 response 를 내려준다.
   1. 사용자면 origin 서버에 그냥 넘겨준다.

<br><br>

## SEO 도 가능할까?

결론: 애매하다.

검색엔진의 알고리즘에 따라 다를 수 있다.

대부분 시맨틱 태그, Body 내부의 아티클이 필요하다. 이런 설정도 가능할 수는 있겠지만 CF의 설정이 너무 복잡해질 것이다. (배보다 배꼽이 더 커질 것이다.)