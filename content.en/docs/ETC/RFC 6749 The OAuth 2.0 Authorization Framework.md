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

# 2. Client Registration

## 2.1 Client Types

OAuth defines two client types, based on their ability to authenticate securely with the authorization server(i.e., ability to maintain the confidentiality of their client credentials):

| Client Type | Description |
| ----------- | ----------- |
| Confidential | Clients capable of maintaining the confidentiality of their credentials, or capable of secure client authentication using other means. |
| Public | Clients incapable of maintaining the confidentiality of there credentials, and incapable of secure client authentication via any other means. |

The client type designation is based on the authorization server's definition of secure authentication requirements and its acceptable exposure levels of client credentials. ⭐ The authorization server SHOULD NOT make assumptions about the client type.

> See also https://letsmakemyselfprogrammer.tistory.com/103

This specification has been designed around the following client profiles:

| Client Profile | Description                                                                                                                                                                                                                                                                                                                                                          |
| -------------- |----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Web Application | A web application is a **confidential client** running on a web server.                                                                                                                                                                                                                                                                                              |
| User Agent Based Application | A user-agent-based application is a **public client** in which the client code is downloaded from a web server and executes within a user-agent (e.g., web browser).                                                                                                                                                                                                 |
| Native Application | A native application is a **public client** installed and executed on the device used by the resource owner.  On the other hand, dynamically issued credentials such as access tokens or refresh tokens can receive an acceptable level of protection. At a minimum, these credentials are protected from hostile servers with which the application ma interact. 🤔 |

## 2.2 Client Identifier

The authorization server issues the client identifier, a unique string representing the registration information provided by the client. The client identifier is not a secret and is unique to the authorization server.

The client identifier string size is left undefined by this specification. The authorization server SHOULD document the size of any identifier it issues. The client should avoid making assumptions about the identifier size.

## 2.3 Client Authentication

If the client type is **confidential**, the client and authorization server establish a client authentication method suitable for the security requirements of the authorization server.

Confidential clients are typically issued a set of client credentials used for authenticating with the authorization server (e.g., password or public/private key pair).

The authorization server MAY establish a client authentication method with public clients. However, the authorization server MUST NOT rely on public client authentication for the purpose of identifying the client. ⭐

The client MUST NOT use more than one authentication method in each request.

### [2.3.1 Client Password](https://datatracker.ietf.org/doc/html/rfc6749#section-2.3.1)

Clients in possession of a client password MAY use the HTTP Basic authentication scheme.

The authorization server MUST support the HTTP Basic authentication scheme for authenticating clients that were issued a client passwrd.

Since this client authentication method involves a password, the authorization server MUST protect any endpoint utilizing it against brute force attacks.

### 2.3.2 Other Authentication Methods

The authorization server MAY support any suitable HTTP authentication scheme matching its security requirements.  **When using other authentication methods, the authorization server MUST define a mapping between the client identifier and authentication scheme.** ⭐

# 3. Protocol Endpoints

About Authorization Endpoint, Token Endpoint, Redirection Endpoint, etc.

_skip_

# 4. Obtaining Authorization

OAuth defines for grant type: authorization code, implicit, resource owner password credentials, and client credentials.A

It also provides an extension mechanism for defining additional grant types.

_skip_

