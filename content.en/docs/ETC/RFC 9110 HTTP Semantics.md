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

# Status Codes

## 15.5 Client Error 4xx

### 15.5.1 400 Bad Request

The server cannot or will not process the request due to something that is perceived to be a client error (e.g., malformed request syntax, invalid request message framing, or deceptive request routing).

### 15.5.2 401 Unauthorized

The request has not been applied because it lacks valid authentication credentials for the target resource.

The server generating a 401 response **MUST** send a **WWW-Authenticate** header containing at least one challenge applicable to the target resource.

### 15.5.4 403 Forbidden

The server understood the request but refuses to fulfill it. 

**A server that wishes to make public why the request has been forbidden can describe that reason in the response content (if any).**

> 응답에 forbidden 의 이유를 설명할 수 있다.

An origin server that wishes to "hide" the current existence of a forbidden target resource **MAY** respond with a status code (404 Not Found).

> 'hiding' 목적으로 404 가 사용될 수 있다.

### 15.5.5 404 Not Found

The origin server did not find a current representation for the target resource or **is not willing to disclose that one exists.**

A 404 status code does not indicate whether this lack of representation is temporary or permanent.

> 'hiding' 목적으로 사용될 수 있다.

### 15.5.6 405 Method Not Allowed

The origin server **MUST** generate an Allow header field in a 405 response containing a list of the target resource's currently supported methods.

### 15.5.10 409 Conflict

The request could not be completed due to a conflict with the current state of the target resource.

The server **SHOULD** generate content that includes enough information for a user to recognize the source of the conflict.

Conflicts are most likely to occur in response to a PUT request.

### 15.5.16 415 Unsupported Media Type

The origin server is refusing to service the request because the content is in a format not supported by this method.

The format problem might be due to the request's indicated **Content-Type** or **Content-Encoding**, **Media-Type(Accept)**, or as a result of inspecting the data directly.

If the problem was caused by an unsupported content coding, the **Accept-Encoding** response header field ought to be used to indicate which (if any) content codings would have been accepted in the request.

On the other hand, if the cause was an unsupported media type, **the Accept response header field can be used to indicate which media types would have been accepted in the request.**

<br>

## 15.6 Server Error 5xx

### 15.6.1 500 Internal Server Error

The server encountered an unexpected condition that prevented it from fulfilling the request.

### 15.6.3 502 Bad Gateway

The server, **while acting as a gateway or proxy, received an invalid response from an inbound server** it accessed while attempting to fulfill the request.

### 15.6.4 Service Unavailable

The server is currently unable to handle the request due to a temporary overload or scheduled maintenance, which will likely be alleviated after some delay.

The server **MAY** send a **Retry-After** header field to suggest an appropriate amount of time for the client to wait before retrying the request.

**Note:** The existence of the 503 status code dose not imply that a server has to use it when becoming overloaded. Some servers might simply refuse the connection.

> 과부하 상태에서 무조건 503 상태 코드를 반환하는 것을 의미하는 게 아니다. 과부하 상태에서 서버는 커넥션을 끊어 요청을 거부할 수 있다.

### 15.6.5 504 Gateway Timeout

The server, while acting as a gateway or proxy, did not receive a timely response from an upstream server it needed to access in order to complete the request.