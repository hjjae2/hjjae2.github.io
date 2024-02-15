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

# What is SSO?

[wiki](https://en.wikipedia.org/wiki/Single_sign-on) 에 설명이 잘 되어 있다.

SSO is an authentication scheme that allows a user to log in with a single ID to any of serveral ralated, yet inependent, software systems.

**True SSO allows the user to log in once and access services without re-entering authentication factors.**

A simple version of SSO can be achieved using cookies but only if the sites share a common DNS parent domain.

> 쿠키 기반의 구현도 간단한 버전의 SSO 구현체로 볼 수 있다. :star:

Same-Sign On and Single-Sign On are different concepts.

# SSO Implementation Models

[SSO 구현 모델 (wiki)](https://wiki.wikisecurity.net/wiki:sso#sso_%EA%B5%AC%ED%98%84_%EB%AA%A8%EB%8D%B8) 문서를 참고해봐도 좋겠다.

> 외국 문서 중에서는 이렇게 구현 모델을 나눈 문서를 못찾았다. 그렇기에 표준으로 정의된 내용이라기 보단 단순히 정리한 정도로 인식하면 좋겠다.

|항목|주요 기능|
|-|-|
|SSO Delegation Model|- SSO 대상 애플리케이션에서 사용되는 인증 방식을 별도의 SSO Agent 가 대신 수행해주는 방식 <br/> - 대상 애플리케이션의 인증 방식을 변경하기 어려운 경우 많이 사용된다. <br/> - 대상 애플리케이션의 인증 방식을 전혀 변경하지 않고, 대상 애플리케이션 인증 정보를 에이전트가 관리해 사용자 대신 로그인 해주는 방식이다. <br/> - Alice 라는 사용자가 Target Service A 에 로그인할 때 alice/alice 라는 ID/PW 가 필요하다면, 에이전트가 이 정보를 갖고 있고, Alice 가 Target Service A 에 접근할 때 에이전트가 대신 로그인을 수행해준다. (에이전트가 사전에 ID/PW 정보를 갖고 있기 때문에 대신 수행해줄 수 있다.)
|SSO Propagation Model|- 통합 인증을 수행하는 곳에서 인증을 수행하고 토큰 등을 발급해 대상 애플리케이션에 전달, 대상 애플리케이션에서 사용자를 확인할 수 있도록 하는 방법이다. <br/> - 웹 환경에서는 쿠키를 통해 토큰을 대상 애플리케이션에 전달할 수 있다. <br/> - 웹 환경에서의 SSO 는 대부분 이 모델을 채택하고 있다.|
|Delegation & Propagation 혼합 Model|- 하나의 Model 로 모든 상황을 해결할 수는 없다. 경우에 따라 혼용해서 구성하기도 한다. <br/> 예를 들어, 웹 환경에서 구현하되(Propagtion) 사용자 인증 방식 변경이 어려운 경우(Delegation)와 같이 다양한 상황이 있을 수 있다.|
|(Web 기반) One Cookie Domain SSO|- SSO 인증 서비스와 대상 애플리케이션들이 하나의 도메인으로 구성될 때 사용되는 가장 일반적인 방식이다. 특히, 기업 내부에서 많이 사용된다. <br/> - 통합 인증을 받은 사용자는 토큰을 발급 받고, 이 토큰은 쿠키에 저장된다. 사용자가 다른 서비스에 접근할 때 (동일한 도메인임으로)이 쿠키를 활용해 대상 애플리케이션에서 사용자의 신원을 확인할 수 있다.|
|(Web 기반) Multi Cookie Domain SSO|- SSO 인증 서비스와 대상 애플리케이션들이 여러 도메인으로 구성돼 있을 경우다. <br/> - Multi Domain 의 경우 사용자 인증, 토큰 발행을 위한 마스터 에이전트가 존재한다. (마스터 에이전트란 통합 인증을 수행하는 IDP 정도로 봐도 좋겠다.) <br/> - 마스터 에이전트는 각 서비스 에이전트(SP 정도로 해석해도 좋겠다.)의 사용자 인증을 위임 받아 수행하며 인증된 사용자에게는 토큰을 발급하고 서비스 에이전트에 전달한다. <br/> - 각 서비스 에이전트는 전달 받은 토큰을 Cookie 로 저장해 해당 도메인에서 사용할 수 있도록 한다. <br/> - 각 서비스 에이전트의 신뢰도 및 SSO 시스템 보안 레벨에 따라 두 방식으로 구현할 수 있다. (`One Token for All Multi Cookie Domain`, `Token for each cookie domain & One Token for master agent`)|

> SSO Delegation, Propagation Model 에 대해서는 https://gruuuuu.github.io/security/ssofriends/ 여기에 잘 설명되어 있다.

다음과 같이 정리한 내용도 있다. (https://alleged.org.uk/pdc/2007/08/13.html)

- Simplest Case: Shared Host
- Simple Case: Shared Domain
- Complicated Case: Single-Sign-On Server


# 참고

- [SSO (wiki)](https://en.wikipedia.org/wiki/Single_sign-on)
- [SSO 구현 모델 (wiki)](https://wiki.wikisecurity.net/wiki:sso#sso_%EA%B5%AC%ED%98%84_%EB%AA%A8%EB%8D%B8)
- https://gruuuuu.github.io/security/ssofriends/
- https://alleged.org.uk/pdc/2007/08/13.html