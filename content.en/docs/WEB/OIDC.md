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

# 1. Introduction

클라이언트가 **인가 서버에 의해 인증이 수행된 최종 사용자(이하 사용자)의 신원을 검증할 수 있게 하고 사용자의 기본 프로필 정보를 교환할 수 있게 한다.** 사용자 정보 교환은 상호 운용 가능하고 REST와 유사한 방식으로 이루어질 수 있다.

OIDC Core 1.0 사양은 다음과 같은 핵심 기능을 정의한다.

1. OAuth 2.0 기반 인증
2. 사용자 정보를 전달하기 위한 Claims 사용

OIDC 는 OAuth 2.0 인증 프레임워크를 확장하여 인증을 제공한다. **이 확장은 클라이언트가 인증 요청에 `openid` 스코프 값을 포함하여 요청하는 것으로 이루어진다. 인증에 대한 정보는 `ID Token`이라는 JSON Web Token(JWT)에
포함되어 반환된다**. ⭐

# 2. ID Token

**OIDC 가 OAuth 2.0을 확장하여 사용자 인증을 가능하게 하는 주요 확장은 `ID Token` 데이터 구조이다.** ⭐ 앞서 말했듯이, `ID Token`은 JSON Web Token(JWT)로 표현된다.

<br> 다음은 `ID Token` 내 사용되는 Calim 목록이다.

| Name      | Description                                                                                                                                                                                                                                   | Required / Optional                                                                                                                                                                                       |
|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| iss       | Issuer Identifier for the Issuer of the response.                                                                                                                                                                                             | Required                                                                                                                                                                                                  |
| sub       | Subject Identifier. **A locally unique and never reassigned identifier within the Issuer for the End-User**, which is intended to be consumed by the client, e.g., `244003` or `AItOawa...`                                                   | Required                                                                                                                                                                                                  |
| aud       | Audience(s) that this ID Token is intended for. It MUST contain the OAuth 2.0 `client_id` of the Relying Party as an audience value.                                                                                                          | Required                                                                                                                                                                                                  |
| exp       | **Expiration time on or after which the ID Token MUST NOT be accepted by the RP** when performing authentication with the OP.                                                                                                                 | Required                                                                                                                                                                                                  |
| iat       | Time at which the JWT was issued. Its value is a JSON number representing the number of seconds from 1970-01-01T00:00:00Z as measured in **UTC** until the date/time.                                                                         | Required                                                                                                                                                                                                  |
| auth_time | Time when the End-User authentication occurred. Its value is a JSON number representing the number of seconds from 1970-01-01T00:00:00Z as measured in **UTC** until the date/time.                                                           | Optional \n\n * When a `max_age` request is made or when `auth_time` is requested as an Essential Claim, then this Claim is REQUIRED; otherwise, its inclusion is OPTIONAL.                               |
| nonce     | String value used to associate a Client session with an ID token, and to mitigate replay attacks.                                                                                                                                             | Optional \n\n * If present in the Authentication Request, Authorization Servers MUST include a nonce Claim in the ID Token with the Claim Value being the nonce value sent in the Authentication Request. |
| acr       | Authentication Context Class Reference.                                                                                                                                                                                                       | Optional                                                                                                                                                                                                  |
| amr       | Authentication Methods References. JSON array of strings that are identifiers for authentication methods used in the authentication. For instance, values might indicate that both **password** and **OTP** authentication methods were used. | Optional                                                                                                                                                                                                  |
| azp       | Authorized party, the party to which the ID Token was issued. If present, it MUST contain the OAuth 2.0 Client ID of this party.                                                                                                              | Optional                                                                                                                                                                                                  |

`ID Token` 은 JWS 를 사용해 **서명**되어야 하며, 선택적으로 JWS 와 JWE 를 사용해 **서명 및 암호화**될 수 있다. 이는 인증, 무결성, 부인 방지, 선택적으로 기밀성을 제공한다. 만약 `ID Token`이 암호화되어 있다면, 서명 후
암호화되어야 하며, 결과는 중첩 JWT 형태로 정의된다.

# 3. Authentication

인증 결과는 ID Token에 포함되어 반환된다. 인증은 다음 세 가지 흐름(flow)중 하나를 따른다.

1. Authorization Code Flow (response_type=code)
2. Implicit Flow (response_type=id_token token or response_type=id_token)
3. Hybrid Flow (using other Response Type values defined in OAuth 2.0 Multiple Response Type Encoding Practices)

**이 흐름(flow)은 ID Token과 Access Token이 클라이언트에 반환되는 방식을 결정한다.** ⭐

| Property                                        | Authorization Code Flow | Implicit Flow | Hybrid Flow |
|-------------------------------------------------|-------------------------|---------------|-------------|
| All tokens returned from Authorization Endpoint | ❌                       | ✅             | ❌           |
| All tokens returned from Token Endpoint         | ✅                       | ❌             | ❌           |
| Tokens not revealed to User Agent               | ✅                       | ❌             | ❌           |
| Client can be authenticated                     | ✅                       | ❌             | ✅           |
| Refresh Token possible                          | ✅                       | ❌             | ✅           |
| Communication in one round trip                 | ❌                       | ✅             | ❌           |
| Most communication server-to-server             | ✅                       | ❌             | varies      |

> * Hybrid Flow 란? Authorization Endpoint에서 Authorization Code가 반환되고, 일부 토큰은 Authorization Endpoint에서 반환되고, 나머지는 Token Endpoint에서 반환되는 OAuth 2.0 흐름(
    flow)이다.


흐름(flow)은 Authorization Request에 포함된 `response_type` 값에 의해 결정된다. 이 `response_type` 값은 다음 흐름(flow)을 선택한다.

| `response_type`     | Flow                    |
|---------------------|-------------------------|
| code                | Authorization Code Flow |
| id_token            | Implicit Flow           |
| id_token token      | Implicit Flow           |
| code id_token       | Hybrid Flow             |
| code token          | Hybrid Flow             |
| code id_token token | Hybrid Flow             |

## 3.1 Authentication using the Authorization Code Flow

이 흐름(flow)은 어떠한 토큰도 User Agent, 악의적인 애플리케이션에 노출하지 않는 이점이 있다. **Authorization Server는 Authorization Code를 Access Token으로 교환하기 전에 Client를 인증할 수 있다.**
즉, Authorization Code Flow는 Client와 Authorization Server 사이에 Client Secret을 안전하게 유지할 수 있는 Client(= Confidential Client)에 적합하다. ⭐

### 3.1.1 Authorization Code Flow Steps

The Authorization Code Flow goes through the following steps.

1. Client prepares an Authentication Request containing the desired request parameters.
2. Client sends the request to the Authorization Server.
3. Authorization Server Authenticates the End-User.
4. Authorization Server obtains End-User Consent/Authorization.
5. Authorization Server sends the End-User back to the Client with an Authorization Code.
6. Client requests a response using the Authorization Code at the Token Endpoint.
7. **Client receives a response that contains an ID Token and Access Token in the response body.** ⭐
8. Client validates the ID token and retrieves the End-User's Subject Identifier.

### 3.1.2 Authorization Endpoint

The Authorization Endpoint performs Authentication of the End-User.

#### 3.1.2.1 Authentication Request

| Name          | Description                                                                                                                                                                                                                                                                    | Required / Recommended / Optional |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------|
| scope         | OIDC requests MUST contain the `openid` scope value. Other scope values MAY be present.                                                                                                                                                                                        | Required                          |
| response_type | OAuth 2.0 Response Type value that determines the authorization processing flow to be used, including what parameters are returned from the endpoints used. When using the Authorization Code Flow, the value is `code`.                                                       | Required                          |
| client_id     | **Client Identifier valid at the Authorization Server.**                                                                                                                                                                                                                       | Required                          |
| redirect_uri  | Redirection URI to which the response will be sent. **This URI MUST exactly match one of the Redirection URI values for the Client pre-registered at the OpenID Provider**, with the matching performed as described in Section 6.2.1 of [RFC3986] (Simple String Comparison). | Required                          |
| state         | Opaque value used to maintain state between the request and the callback. Typically, CSRF mitigation is done by cryptographically binding the value of this parameter with a browser cookie.                                                                                   | Recommended                       |
| response_mode | Informs the Authorization Server of the mechanism to be used for returning parameters from the Authorization Endpoint.                                                                                                                                                         | Optional                          |
| nonce         | String value used to associate a Client session with an ID Token, and to mitigate replay attacks                                                                                                                                                                               | Optional                          |
| display       | ASCII string value that specifies how the Authorization Server displays the authentication and consent UI pages to the End-User. \n\n The defined values are page, popup, touch, wap                                                                                           | Optional                          |
| prompt        | Space-delimited, case-sensitive list of ASCII string values that specifies whether the Authorization Server prompts the End-User for reauthentication and consent. \n\n The defined values are none, login, consent, select_account.                                           | Optional                          |
| max_age       | Maximum Authentication Age. Specifies the allowable elapsed time in seconds since the last time the End-User was actively authenticated by the OP. If the elapsed time is greater than this value, the OP MUST attempt to actively re-authenticate the End-User.               | Optional                          |
| ui_locales    | End-User’s preferred languages and scripts for the UI, represented as a space-separated list of BCP47[RFC5646] language tag values, ordered by preference.                                                                                                                     | Optional                          |
| id_token_hint | ID Token previously issued by the Authorization Server being passed as a hint about the End-User’s current or past authenticated session with the Client.                                                                                                                      | Optional                          |
| login_hint    | Hint to the Authorization Server about the login identifier the End-User might use to log in (if necessary).                                                                                                                                                                   | Optional                          |
| acr_values    | Requested Authentication Context Class Reference values.                                                                                                                                                                                                                       | Optional                          |

다음은 요청 예시이다.

```
GET /authorize?
    response_type=code
    &scope=openid%20profile%20email
    &client_id=s6BhdRkqt3
    &state=af0ifjsldkj
    &redirect_uri=https%3A%2F%2Fclient.example.org%2Fcb HTTP/1.1
Host: server.example.com
```

### 3.1.3 Token Endpoint

**통신은 TLS를 사용해야 한다.** ⭐

#### 3.1.3.1 Token Request

클라이언트는 (Authorization Code 인증 요청에서) Authorization Grant 를 Token Endpoint에 제출해 토큰 발급(Token Request)을 요청해야 한다. 이때, `grant_type` 값은 `authorization_code`
여야 한다. ⭐

**만약, 클라이언트가 Confidential Client라면, `client_id`에 대한 등록된 인증 방법을 사용해 Token Endpoint에 인증해야 한다. ⭐**

<br/> 다음은 요청 예시이다.
```
POST /token HTTP/1.1
Host: server.example.com
Content-Type: application/x-www-form-urlencoded
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW // Confidential Client, s6BhdRkqt3:gX1fBat3bV

grant_type=authorization_code
&code=SplxlOBeZQQYbYS6WxSbIA
&redirect_uri=https%3A%2F%2Fclient.example.org%2Fcb

```

## 3.2 Authentication using the Implicit Flow

Implicit Flow 에서는, 모든 토큰은 Authorization Endpoint 에서 구할 수 있다. Token Endpoint 는 사용되지 않는다.

Implicit Flow 는 스크립트 언어를 사용하는 브라우저 기반 클라이언트에서 주로 사용된다. Access Token, ID Token 은 클라이언트에 직접 반환된다. Authorization Server 는 클라이언트 인증을 수행하지 않는다.

정리하면 Authorization Code Flow 는 Credential Client 에, Implicit Flow 는 Public Client 에 적합하다고 볼 수 있겠다. ⭐

### 3.2.1 Implicit Flow Steps

The Implicit Flow follows the following steps:

1. Client prepares an Authentication Request containing the desired request parameters.
2. Client sends the request to the Authorization Server.
3. Authorization Server Authenticates the End-User.
4. Authorization Server obtains End-User Consent/Authorization.
5. Authorization Server sends the End-User back to the Client with **an ID Token and, if requested, an Access Token.**
5. Client validates the ID token and retrieves the End-User's Subject Identifier.

### 3.2.2 Authorization Endpoint

3.2.1 섹션에서 설명한 Authorization Endpoint 와 동일하다.

#### 3.2.2.1 Authentication Request

Authentication Request 는 Authorization Code Flow(섹션 3.1.2.1) 와 동일하다. 다만, 파라미터에 차이가 있다.

| Name          | Description                                                                                                                      
                                                                                                                                              | Required / Recommended / Optional | 
|---------------|------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------|
| response_type | Implicit Flow 에서 이 값은 `id_token token` 이거나 `id_token` 이다. `id_token` 인 경우, Access Token 은 반환되지 않는다. OAuth2.0 에서 Implicit Flow 에서 
`token` 값이 사용되기도 하지만 OIDC 에서는 사용되지 않는다. | Required                          |
| redirect_uri | Authorization Code Flow 와 동일하다. 다만, scheme 은 반드시(MUST) `https`를 사용해야 한다. Native Application(= localhost?) 인 경우에만 `http` 를 허용할 수 
있으나 `localhost`, `127.0.0.1`, `[::1]` 만 허용된다. 참고로 Authorization Code 에서 `https` 사용은 권고 사항(SHOULD)이었다. 클라이언트가 Native Application 이거나 혹은 클라이언트가 기밀
(confidential) 타입이고 Provider 가 http 를 지원하는 가정 하에 http 를 사용할 수 있었다. | 
Required                          |
| nonce | String value used to associate a Client session with an ID Token, and to mitigate replay attacks. | Required                          |
    
<br>다음은 요청 예시이다.
```

GET /authorize?
response_type=id_token%20token
&client_id=s6BhdRkqt3
&redirect_uri=https%3A%2F%2Fclient.example.org%2Fcb
&scope=openid%20profile
&state=af0ifjsldkj
&nonce=n-0S6_WzA2Mj HTTP/1.1
Host: server.example.com

```

#### 3.2.2.2 Authentication Request Validation

Authorization Code Flow 와 동일하다.

#### 3.2.2.3 Authorization Server Authenticates End-User

Authorization Code Flow 와 동일하다.

#### 3.2.2.4 Authorization Server Obtains End-User Consent/Authorization

Authorization Code Flow 와 동일하다.

#### 3.2.2.5 Successful Authentication Response

Authorization Code Flow 와 동일한 방식으로 Authentication Response 가 반환된다. 다만, (특별히 Response Mode 가 설정되어 있지 않으면)모든 Response Parameter 들은 Redirection URI 의 프래그먼트(fragment)에 포함되어 반환된다.

반환되는 파라미터 목록은 다음과 같다.

| Name          | Description | Required / Optional |
|---------------|-------------| ------------------- |
| access_token  | Access Token | Optional |
| token_type    | 이 값은 `Bearer` 여야 한다. 혹은 클라이언트가 Authorization Server 와 협상(negotiation)한 값이어야 한다. | Optional | 
| id_token      | ID Token | Required |
| state | Authorization Request 에 포함되었다면 필수로 반환된다. 클라이언트는 요청에 보낸 state 값과 동일한지 확인해야 한다. | Optional |
| expires_in | Access Token 의 만료 시간(Expiration Time)을 초 단위로 나타낸 값이다. | Optional |

<br>다음은 응답 예시이다.
```

HTTP/1.1 302 Found
Location: https://client.example.org/cb#
access_token=SlAV32hkKG
&token_type=bearer
&id_token=eyJ0 ... NiJ9.eyJ1c ... I6IjIifX0.DeWt4Qu ... ZXso
&expires_in=3600
&state=af0ifjsldkj

```

#### 3.2.2.6 Authentication Error Response

Authorization Code Flow 와 동일한 방식으로 처리된다. 다만, Successful Authentication Response 와 동일하게 응답 값은 Redirection URI 의 프래그먼트(fragment)에 포함되어 반환된다.

#### 3.2.2.10 ID Token

Implicit Flow 에서 ID Token 의 클레임에 추가 요구 사항(requirements)이 적용된다.

| Name          | Description |
|---------------|-------------|
| nonce | Use of the nonce Claim is REQUIRED for this flow. |
| at_hash | Access Token hash value. |

#### 3.2.2.11 ID Token Validation

기본적으로 Implicit Flow 에서 ID Token 의 검증은 Authorization Code Flow 와 동일하다.다만, 아래의 차이점이 있다.

1. 클라이언트는 ID Token의 서명을 반드시(MUST) 검증해야 한다. 
2. 클라이언트는 `nonce` 값이 Authentication Request 에서 전송된 값과 동일한지 반드시(MUST) 검증해야 한다. The Client SHOULD check the nonce value for replay attacks. The precise method for detecting replay attacks is Client specific.

## 3.3 Authentication using the Hybrid Flow

몇몇의 토큰은 Authorization Endpoint 에서 구할 수 있고, 나머지는 Token Endpoint 에서 구할 수 있다.

### 3.3.1 Hybrid Flow Steps

The Hybrid Flow follows the following steps:

1. Client prepares an Authentication Request containing the desired request parameters.
2. Client sends the request to the Authorization Server.
3. Authorization Server Authenticates the End-User.
4. Authorization Server obtains End-User Consent/Authorization.
5. Authorization Server sends the End-User back to the Client with an Authorization Code and, **depending on the Response Type, one or more additional parameters.**
6. Client requests a response using the Authorization Code at the Token Endpoint.
7. Client receives a response that contains an ID Token and Access Token in the response body.
8. Client validates the ID Token and retrieves the End-User's Subject Identifier.

**Authorization Code FLow Steps 와 동일한 것을 알 수 있다. 유일한 차이는 5번째 단계에서 Response Type에 따라 추가적인 파라미터가 반환될 수 있다는 것이다.** ⭐

response_type 에 `id_token`이 포함되었기면 id_token 도 반환된 것 같다만, 그럼 7번 단계에서 id_token 을 또 받는건가? 🤔

#### 3.3.2.1 Authentication Request

Authentication Request 는 Authorization Code Flow 와 동일하다. 다만, 파라미터에 차이가 있다.

| Name          | Description | Required / Recommended / Optional |
|---------------|-------------|-----------------------------------|
| response_type | `code id_token`, `code token`, `code id_token token` 중 하나여야 한다. | Required     |
| nonce | 만약 Response Type 이 `code_id token`, `code id_token token` 이면 Required 이다. `code token` 인 경우, 즉 `id_token` 이 없는 경우는 Optional 이다. | Required / Optional | 

<br>다음은 요청 예시이다.
```

GET /authorize?
response_type=code%20id_token
&client_id=s6BhdRkqt3
&redirect_uri=https%3A%2F%2Fclient.example.org%2Fcb
&scope=openid%20profile%20email
&nonce=n-0S6_WzA2Mj
&state=af0ifjsldkj HTTP/1.1
Host: server.example.com

```

#### 3.3.2.5 Successful Authentication Response

Implicit Flow 와 동일한 방식으로 Authentication Response 가 반환된다. 다만, 아래 차이점이 있다.

| Name          | Description | Required / Optional |
|---------------|-------------|----------------------|
| access_token  | `code token` 이나 `code id_token token` 인 경우에만 반환된다. | Optional |
| token_type    | `code id_token`, `code id_token token` 인 경우에만 반환된다. | Optional |
| code          | Authorization Code | Required |

<br>다음은 응답 예시이다.
```
HTTP/1.1 302 Found
Location: https://client.example.org/cb#
code=SplxlOBeZQQYbYS6WxSbIA
&id_token=eyJ0 ... NiJ9.eyJ1c ... I6IjIifX0.DeWt4Qu ... ZXso
&state=af0ifjsldkj
```

# 4. Initiating Login from a Third Party

몇몇의 경우에는, 로그인 흐름(flow)은 OpenID Provider나 다른 당사자(party)에 의해 시작된다. 이때, 로그인 흐름(flow)은 RP의 기본 랜딩 페이지와 다른 페이지일 수 있다. RP는 OpenID Connect Dynamic Client Registration 1.0을 지원하는 경우, `initiate_login_uri` Registration parameter를 사용해 이 엔드포인트 값을 등록한다.

로그인 요청을 시작하는 당사자(party)는 다음과 같은 매개변수를 전달하며 RP의 로그인 시작 엔드포인트로 리다이렉트한다.

| Name          | Description | Required / Recommended / Optional |
|---------------|-------------|-----------------------------------|
| iss           | Issuer Identifier for the OP that the RP is to send the Authentication Request to. **Its value MUST be a URL using the `https` scheme.** | Required |
| login_hint    | Hint to the Authorization Server about the End-User to be authenticated. | Optional |
| target_link_uri | URL that the RP is requested to redirect to after authentication. **RPs MUST verify the value of the `target_link_uri` to prevent being used as an open redirector to external sites.** | Optional |

파라미터들은 HTTP GET 요청의 쿼리 파라미터로 전달되거나 HTML 폼 값(HTTP POST)으로 전달된다.

# 5. Claims

클래임의 사전 정의된 집합(sets, 목록)은 (1) 특정 스코프 값들을 사용해 요청하거나 (2) `claims` 요청 파라미터를 사용해 개별 클래임들을 요청할 수 있다.

## 5.1 Standard Claims

이 사양은 표준 클래임 집합을 정의한다. 이들은 UserInfo 응답에 반환되거나 ID Token에 반환될 수 있다.

| Member          | Type   |
|-----------------|--------|
| sub ⭐           | string |
| name ⭐          | string |
| given_name      | string |
| family_name     | string |
| middle_name     | string |
| nickname ⭐       | string |
| preferred_username | string |
| profile ⭐         | string |
| picture         | string |
| website         | string |
| email ⭐          | string |
| email_verified  | boolean |
| gender ⭐         | string |
| birthdate ⭐      | string |
| zoneinfo        | string |
| locale ⭐         | string |
| phone_number ⭐   | string |
| phone_number_verified | boolean |
| address         | JSON object |
| updated_at      | number |


# 99. References

https://openid.net/specs/openid-connect-core-1_0.html
