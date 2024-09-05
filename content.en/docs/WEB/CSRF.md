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

# What is CSRF(Cross Site Request Forgery)?

웹 애플리케이션 취약점 중 하나이다. \
공격자가 자신(피해자)의 권한을 도용해 의도한 행위(수정, 삭제, 등록 등)를 특정 웹사이트에 요청하는 공격이다.

1. 대상이 될 서비스(웹 애플리케이션)에 피해자가 로그인 한 상태이다. 
   * 이때, 정상적인 로그인 쿠키(예시)를 발급 받는다.
2. 피해자가 공격자의 피싱 사이트에 접속한다.
3. 공격자는 피해자의 권한을 도용해 원래의 웹사이트에 요청을 보낸다.

## 방어 기법

일반적으로 CSRF 방어 기법은 조회성 메소드(GET) 에는 적용하지 않고, POST, PUT, PATCH, DELETE 메소드에 중점적으로 적용한다.

### 1. Referrer 검증

일반적으로 referrer 검증만으로 대부분의 CSRF 공격을 방어할 수 있다.
예를 들어, 같은 도메인이 아닌 개인 이메일, 다른 도메인에서 들어오는 referrer 를 차단하는 것이다. XSS 취약점이 있다면, CSRF 공격에 취약해질 수 있다.

### 2. CSRF Token(Security Token) 사용

사용자의 세션에 임의의 토큰(난수 값)을 저장하고, 사용자는 요청 마다 해당 토큰을 포함시켜 전송한다. 서버에서는 요청을 받을 때마다 서버에 저장된 토큰 값과 요청에서 받은 토큰 값을 비교하여 검증할 수 있다.

예를 들어, 서버에서 임의의 토큰(난수 값)을 생성하고 (View)form 쪽에 csrf 토큰을 세팅하여 페이지를 내려준다. 정상적인 페이지라면 해당 토큰 값이 존재하고 유효하기 때문에 성공, 피싱 페이지라면 해당 토큰 값이 존재하지도 않을 수 있고 유효하지 않기 때문에 실패할 것이다.

# References
* https://itstory.tk/entry/CSRF-%EA%B3%B5%EA%B2%A9%EC%9D%B4%EB%9E%80-%EA%B7%B8%EB%A6%AC%EA%B3%A0-CSRF-%EB%B0%A9%EC%96%B4-%EB%B0%A9%EB%B2%95
* https://sj602.github.io/2018/07/14/what-is-CSRF/