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

> https://devocean.sk.com/blog/techBoardDetail.do?ID=164788

## 결론

**1. 언어마다 다를 수 있다.**

**2. 시스템(플랫폼)마다 다를 수 있다.**

<br>

## C/C++ : 4 or 8 Byte

시스템(플랫폼)에 의존적이다.

LLP64, LP64 등 데이터 모델에 따라 다르다.

## Java : 4 Byte 고정

플랫폼에 독립적인 가상머신(JVM)에서 동작하기에 플랫폼의 영향을 받지 않는다.

4Byte 로 고정되어 있다.

## Python : 8 Byte ~ 

Python 의 경우 int 의 크기가 정해져 있지 않다.

> `sys.maxsize` 의 경우, 시스템에 의존적인 형태라고 볼 수 있을 것 같다.

정해진 범위를 초과하더라도 (자동으로)데이터 범위(?)를 증가시켜준다.

<br>

## 참고

![](/images/[DEV]%20int는%20몇%20바이트%20인가요_14.png)