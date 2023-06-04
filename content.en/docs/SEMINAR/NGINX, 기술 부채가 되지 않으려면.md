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

## 로드밸런서
<img src="/images/[DEV]%20NGINX,%20기술%20부채가%20되지%20않으려면_48.png">

> GSLB, LSLB, CSLB

<br>

## 요약

1. Nginx 설정도 **저장소**에서 관리하자.
   1. 배포까지 자동화할 수 있다.
2. **주석**은 꼼꼼히 작성하자.
3. 주기적으로 확인하여 필요하지 않은 설정은 제거하자. (계속해서 관리하자.)

<br>

## mirror

![](/images/[DEV]%20NGINX,%20기술%20부채가%20되지%20않으려면_16.png)

nginx 설정은 업데이트를 한다. 기존 설정이 변경되다보니 항상 걱정하게 된다.

mirror 는 들어온 요청을 그대로 복제해서 또 다른 upstream 서버 / block 으로 보내주는 역할을 한다.

이를 통해 기존 로직을 손대지 않고 설정 테스트가 가능하다. 