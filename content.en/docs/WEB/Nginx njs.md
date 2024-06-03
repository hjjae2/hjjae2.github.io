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

Nginx njs 에 대해 알아보자.

## What is Nginx njs?

njs 는 Nginx 에서 기능의 확장을 위해 실행될 수 있는 Javascript 언어의 'subset' 이다.

쉽게 말하면 Nginx 에서 Javascript 를 사용할 수 있게 해주는 모듈/기능이다.

```nginx configuration
load_module modules/ngx_http_js_module.so;

http {
    js_import http.js;

    server {
        listen 8000;

        location / {
            js_content http.hello;
        }
    }
}
```
```javascript
function hello(r) {
    r.return(200, "Hello world!");
}

export default {hello};
```

ECMAScript 5.1 문법을 사용할 수 있고 ECMAScript 6 문법 중 일부를 지원한다. 모듈(ngx_http_js_module)의 버전에 따라 지원되는 문법이 다르다.

사용할 수 있는 객체(Reference), 메서드도 모듈의 버전에 따라 다르다. 😭

## Examples

https://github.com/nginx/njs-examples/

## References

https://nginx.org/en/docs/njs/

