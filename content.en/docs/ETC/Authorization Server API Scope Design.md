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

## [API Authorization Steps](https://authguidance.com/api-authorization-design/)

1. Token Validation

2. Scope Checks

3. Claims Based Authorization

### **Authorization 확인은 세 가지 단계로 정리할 수 있다.**

### 1. 토큰 유효성

토큰 자체를 검사한다.

- 유효한 토큰인지
- 만료되지 않았는지

### 2. scope 확인

해당 요청(API)이 허용될 수 있는 요청인지 확인한다.

**예시**

| order | order 도메인에 대해 read API를 호출할 수 있다. |
| --- | --- |
| order_write | order 도메인에 대해 write API를 호출할 수 있다. |

### 3. 구체적인 권한 확인

해당 요청(API)이 허용될 수 있는 요청인지 확인한다. 

(2번에서 허용된)API 내에서 확인해야 하는, 조금 더 구체적인 권한 확인을 의미한다.

**예시**

- 사용자 A 가 1번 리소스(자신의 리소스)를 요청한다. ← O
- 사용자 A 가 2번 리소스(타인의 리소스)를 요청한다. ← X

<br>

## [Scope Best Practices (in OAuth 2.0)](https://curity.io/resources/learn/scope-best-practices/)

### Tips for Designing Scope

### 1. Hierarchical Scopes

필요에 따라 계층 구조를 지원하는 scope 을 설계할 수 있다.

### 2. Use Least Privilege Scopes

하나의 scope 가 최소한의 권한만 포함하도록 설계해야 한다. 

단, scope explosion 도 함께 고려해야한다.

### 3. Avoid Scope Explosion

Scope 를 잘못 설계하면 너무 많은 scope 들이 생성되어 관리가 어려워질 수 있다. 

이런 현상을 Scope Explosion 이라고 하는데, 이 현상을 피하기 위해 적절한 설계가 필요하다.

<br>

## Note

### **OAuth, 혹은 다른 토큰 기반의 인가 구조의 흐름은 대략 다음으로 동일하다.**

> 클라이언트(혹은 사용자)가 권한(role, scope)을 부여받았다는 전제하에,

1. 클라이언트는 '인증'을 통해 '토큰'을 획득한다. <br> 토큰에는 클라이언트가 부여받은 '권한'이 명시되어 있다.
2. 클라이언트가 토큰과 함께 API 를 요청한다. <br> **즉, 클라이언트는 자신의 권한을 내밀며 API를 요청한다.**
3. 서버는 권한을 확인하고 올바른 요청이라면 응답, 올바르지 않은 요청이라면 요청을 거절한다.

### **올바른 요청, 올바르지 않은 요청을 확인하는 로직은 어떻게 구현될 수 있을까? (위 3번)**

사전에 권한(scope, ...)과 API Endpoint 를 매핑/관리해두고 확인할 수 있다.

위 로직은,

1. API Endpoint 단에서 수행할 수 있다.
2. 클라이언트 -> Auth Server -> API 처럼 타 컴포넌트가 수행해줄 수 있다.

### **(위 2번 구조에서) 권한 확인 로직에서 API Endpoint 를 사용하지 않고 구현할 수 있을까?**

API Enpoint 는 API URI, '리소스의 식별자'를 의미한다.

Endpoint 가 사용되지 않는다는 것은 리소스의 식별자를 사용하지 않는다는 의미다.

그럼 Endpoint 말고 다른 리소스의 식별자를 사용해야 할 것이다. 

(고유하다는 전제 하에) 리소스의 '이름'과 같은 속성들이 사용될 수 있겠다.

클라이언트가 API를 호출할 때 고유한 속성을 함께 보내주어 확인할 수 있겠다.

> 단, 고유한 속성을 함께 보내주는 것도 '관리'해줘야 한다.
> 
> 예를 들어,
> 
> 클라이언트가 어떤 API 를 호출할 때 어떤 고유한 속성(여기선, 이름)을 함께 보내줘야 하는지 관리하고 인지하고 있어야 한다.
> 
> 이렇게 되면 API 별로 보내줘야 하는 고유한 속성을 관리하게 될텐데, 그럼 다시 Endpoint 를 사용하는 것과 동일하다고 볼 수 있다.
