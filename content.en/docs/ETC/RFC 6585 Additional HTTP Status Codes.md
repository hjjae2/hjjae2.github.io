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

# 4.  429 Too Many Requests

The 429 status code indicates that the user has sent too many requests in a given amount of time ("rate limiting").

The response representations **SHOULD** include details explaining the condition, and **MAY** include a **Retry-After** header indicating how long to wait before making a new request.

For example:
```
HTTP/1.1 429 Too Many Requests
Content-Type: text/html
Retry-After: 3600

<html>
    <head>
        <title>Too Many Requests</title>
    </head>
    <body>
        <h1>Too Many Requests</h1>
        <p>I only allow 50 requests per hour to this Web site per
        logged in user.  Try again soon.</p>
    </body>
</html>
```

Note that this specification does not define how the origin server identifies the user, nor how it counts requests.  

> Rate Limiting 구현부(client 식별, 카운팅 방법 등)는 다루지 않는다.