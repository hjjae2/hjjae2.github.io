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

# [JWT Profile for OAuth 2.0 Client Authentication and Authorization Grants](https://datatracker.ietf.org/doc/html/rfc7523)

> OAuth 2.0 클라이언트 인증, 권한 부여 그랜트를 위한 JWT 프로필에 대해 설명한다.

OAuth 2.0 프로토콜에서 인증, 권한 획득을 위해 JWT를 사용하는 방법에 대한 지침, 규칙을 제시한다.

이를 통해 클라이언트는 JWT를 사용해 자신의 신원을 **인증**하고, 보호된 리소스에 대한 **엑세스 권한**을 획득할 수 있다.

## 1. Introduction

OAuth also allows for the definition of new extension grant types to support additional clients or to provide a bridge between OAuth and other trust frameworks.

"Assertion Framework for OAuth 2.0 Client Authentication and Authorization Grants" [RFC7521] is an abstract extension to OAuth 2.0 that provides a general framework for the use of assertions (a.k.a. security tokens) as client credentials and/or authorization grants with OAuth 2.0. 

This specification profiles the OAuth Assertion Framework [RFC7521] to define an extension grant type that uses a JWT Bearer Token to request an OAuth 2.0 access token as well as for use as client credentials.

**This document defines how a JWT Bearer Token can be used to request an access token when a client wishes to utilize an existing trust relationship, expressed through the semantics of the JWT, without a direct user-approval step at the authorization server.**

**It also defines how a JWT can be used as a client authentication mechanism.**

The use of a security token for client authentication is orthogonal to and separable from using a security token as an authorization grant. They can be used either in combination or separately.

Client authentication using a JWT is nothing more than an alternative way for a client to authenticate to the token endpoint and must be used in conjunction with some grant type to form a complete and meaningful protocol request. 

**JWT authorization grants may be used with or without client authentication or identification.**

Whether or not client authentication is needed in conjunction with a JWT authorization grant, as well as the supported types of client authentication, are policy decisions at the discretion of the authorization server.

> JWT 권한 부여와 함께 클라이언트 인증이 필요한지 여부와 지원되는 유형의 클라이언트 인증은 권한 부여 서버의 재량에 따라 정책 결정됩니다.

The process by which the client obtains the JWT, prior to exchanging it with the authorization server or using it for client authentication, is out of scope.

> JWT를 획득하는 프로세스는 범위를 벗어납니다.


## 2. HTTP Parameter Bindings for Transporting Assertions
The OAuth Assertion Framework [RFC7521] defines generic HTTP parameters for transporting assertions (a.k.a. security tokens) during interactions with a token endpoint.  

This section defines specific parameters and treatments of those parameters for use with JWT Bearer Tokens.

### 2.1 Using JWTs as Authorization Grants

To use a Bearer JWT as an authorization grant, the client uses an access token request as defined in Section 4 of the OAuth Assertion Framework [RFC7521] with the following specific parameter values and encodings.

- **The value of the "grant_type" is "urn:ietf:params:oauth:grant-type:jwt-bearer".**
- **The value of the "assertion" parameter MUST contain a single JWT.**
- **The "scope" parameter may be used, as defined in the OAuth Assertion Framework [RFC7521], to indicate the requested scope.**

Authentication of the client is optional, as described in Section 3.2.1 of OAuth 2.0 [RFC6749] and consequently, the "client_id" is only needed when a form of client authentication that relies on the parameter is used.

> 클라이언트 인증은 OAuth 2.0 [RFC6749]의 섹션 3.2.1에 설명된 바와 같이 선택적이며, 결과적으로 "client_id"는 매개변수에 의존하는 클라이언트 인증확인 양식이 사용되는 경우에만 필요합니다.

The following example demonstrates an access token request with a JWT as an authorization grant (with extra line breaks for display purposes only):

```
     POST /token.oauth2 HTTP/1.1
     Host: as.example.com
     Content-Type: application/x-www-form-urlencoded

     grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer&
     assertion=eyJhbGciOiJFUzI1NiIsImtpZCI6IjE2In0.eyJpc3Mi[...omitted for brevity...].J9l-ZhwP[...omitted for brevity...]
```

### 2.2.  Using JWTs for Client Authentication

To use a JWT Bearer Token for client authentication, the client uses the following parameter values and encodings.

- **The value of the "client_assertion_type" is "urn:ietf:params:oauth:client-assertion-type:jwt-bearer".**
- **The value of the "client_assertion" parameter contains a single JWT. It MUST NOT contain more than one JWT.**


The following example demonstrates client authentication using a JWT during the presentation of an authorization code grant in an access token request (with extra line breaks for display purposes only):

```
     POST /token.oauth2 HTTP/1.1
     Host: as.example.com
     Content-Type: application/x-www-form-urlencoded

     grant_type=authorization_code&
     code=n0esc3NRze7LTCu7iYzS6a5acc3f0ogp4&
     client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&
     client_assertion=eyJhbGciOiJSUzI1NiIsImtpZCI6IjIyIn0.eyJpc3Mi[...omitted for brevity...].cC4hiUPo[...omitted for brevity...]
```

## 3. JWT Format and Processing Requirements

In order to issue an access token response as described in OAuth 2.0[RFC6749] or to rely on a JWT for client authentication, the authorization server MUST validate the JWT according to the criteria below. 

1. The JWT MUST contain an **"iss" (issuer)** claim that contains a unique identifier for the entity that issued the JWT.
   1. In the absence of an application profile specifying otherwise, compliant applications MUST compare issuer values using the Simple String Comparison method defined in Section 6.2.1 of RFC 3986 [RFC3986]. (이해)
2. The JWT MUST contain a **"sub" (subject)** claim identifying the principal that is the subject of the JWT.  Two cases need to be differentiated:
   1. For the authorization grant, the subject typically identifies an authorized accessor for which the access token is being requested (i.e., the resource owner or an authorized delegate), but in some cases, may be a pseudonymous identifier or other value denoting an anonymous user. (이해)
   2. For client authentication, the subject MUST be the "client_id" of the OAuth client.
3. The JWT MUST contain an **"aud" (audience)** claim containing a value that identifies the authorization server as an intended audience.
   1. The token endpoint URL of the authorization server MAY be used as a value for an "aud" element to identify the authorization server as an intended audience of the JWT.
   2. **The authorization server MUST reject any JWT that does not contain its own identity as the intended audience.**
   3. As noted in Section 5, the precise strings to be used as the audience for a given authorization server must be configured out of band by the authorization server and the issuer of the JWT.
4. The JWT MUST contain an **"exp" (expiration time)** claim that limits the time window during which the JWT can be used.
    1. **The authorization server MUST reject any JWT with an expiration time that has passed**, subject to allowable clock skew between systems.  
    2. **Note that the authorization server may reject JWTs with an "exp" claim value that is unreasonably far in the future.**
5. The JWT MAY contain an **"nbf" (not before)** claim that identifies the time before which the token MUST NOT be accepted for processing.
6. The JWT MAY contain an **"iat" (issued at)** claim that identifies the time at which the JWT was issued.  
   1. **Note that the authorization server may reject JWTs with an "iat" claim value that is unreasonably far in the past.**
7. The JWT MAY contain a **"jti" (JWT ID)** claim that provides a unique identifier for the token.  
   1. The authorization server MAY ensure that JWTs are not replayed by maintaining the set of used "jti" values for the length of time for which the JWT would be considered valid based on the applicable "exp" instant.
8. The JWT MAY contain other claims.
9. **The JWT MUST be digitally signed or have a Message Authentication Code (MAC) applied by the issuer.**  
   1. The authorization server MUST reject JWTs with an invalid signature or MAC.
10. The authorization server MUST reject a JWT that is not valid in all other respects per "JSON Web Token (JWT)"


### 3.1. Authorization Grant Processing

JWT authorization grants may be used with or without client authentication or identification.

However, if client credentials are present in the request, the authorization server MUST validate them. :thinking:

If the JWT is not valid, or the current time is not within the token's valid time window for use, the authorization server constructs an error response as defined in OAuth 2.0 [RFC6749].  

- The value of the "error" parameter MUST be the "invalid_grant" error code.
- The authorization server MAY include additional information regarding the reasons the JWT was considered invalid using the "error_description" or "error_uri" parameters.

For example:

```
     HTTP/1.1 400 Bad Request
     Content-Type: application/json
     Cache-Control: no-store

     {
      "error":"invalid_grant",
      "error_description":"Audience validation failed"
     }
```

### 3.2. Client Authentication Processing

- The value of the "error" parameter MUST be the "invalid_client" error code.  
- The authorization server MAY include additional information regarding the reasons the JWT was considered invalid using the "error_description" or "error_uri" parameters.

## 4. Authorization Grant Example

The example shows a JWT issued and signed by the system entity identified as "https://jwt-idp.example.com".

The subject of the JWT is identified by email address as "mike@example.com".  

The intended audience of the JWT is "https://jwt-rp.example.net", which is an identifier with which the authorization server identifies itself.

 The JWT is sent as part of an access token request to the authorization server's token endpoint at "https://authz.example.net/token.oauth2".

Below is an example JSON object that could be encoded to produce the JWT Claims Set for a JWT:

```
{
    "iss":"https://jwt-idp.example.com",
    "sub":"mailto:mike@example.com",
    "aud":"https://jwt-rp.example.net",
    "nbf":1300815780,
    "exp":1300819380,
    "http://claims.example.com/member":true
}
```

The following example JSON object, used as the header of a JWT, declares that the JWT is signed with the Elliptic Curve Digital Signature Algorithm (ECDSA) P-256 SHA-256 using a key identified by the "kid" value "16".

```
{"alg":"ES256","kid":"16"}
```

To present the JWT with the claims and header shown in the previous example as part of an access token request, for example, the client might make the following HTTPS request (with extra line breaks for display purposes only):
```
     POST /token.oauth2 HTTP/1.1
     Host: authz.example.net
     Content-Type: application/x-www-form-urlencoded

     grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
     &assertion=eyJhbGciOiJFUzI1NiIsImtpZCI6IjE2In0.
     eyJpc3Mi[...omitted for brevity...].
     J9l-ZhwP[...omitted for brevity...]
```

## 5. Interoperability Considerations

Agreement between system entities regarding identifiers, keys, and endpoints is required in order to achieve interoperable deployments of this profile.

Specific items that require agreement are as follows: 
- values for the issuer and audience identifiers
- the location of the token endpoint
- the key used to apply and verify the digital signature or MAC over the JWT
- one-time use restrictions on the JWT
- maximum JWT lifetime allowed
- the specific subject and claim requirements of the JWT

The exchange of such information is explicitly out of scope for this specification.

## 7. Privacy Considerations

1. A JWT may contain privacy-sensitive information and, to prevent disclosure of such information to unintended parties, should only be transmitted over encrypted channels, such as Transport Layer Security(TLS).  
2. In cases where it is desirable to prevent disclosure of certain information to the client, the JWT should be encrypted to the authorization server.
3. Deployments should determine the minimum amount of information necessary to complete the exchange and include only such claims in the JWT.  
   1. In some cases, the "sub" (subject) claim can be a value representing an anonymous or pseudonymous user, as described in Section 6.3.1 of the OAuth Assertion Framework [RFC7521].
