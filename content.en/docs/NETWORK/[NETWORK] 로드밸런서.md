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

먼저 ['L4/L7 로드밸런싱 쉽게 이해하기'](https://aws-hyoh.tistory.com/149) 블로그의 글을 읽었다.

L4/L7 로드밸런싱에 대해서 놓치기 쉬운 것(오해하기 쉬운 것)은 다음과 같다고 한다.

**상위 계층의 장비는 하위 계층의 기능을 포함한다. 다만 효율성 있게 사용하기 위해 사용하지 않을 뿐이다.**

<br>

## 로드밸런서

로드밸런서의 종류는 L2(MAC addr), L3(IP), L4(Port), L7(Application) 로드밸런서가 있다고 한다. 이들 중 L4, L7 로드밸런서가 많이 사용된다고 한다. (\* L4부터 Port 를 다룰 수 있는데, 이 말은 즉슨 한 대의 서버에 여러 포트로 여러 서비스를 운영할 수 있다는 것을 의미한다.) 예전에는 L4 위주의 장비를 많이 사용했다고 한다면, 요새는 MSA 의 추세로 L7 장비도 많이 사용되고 있다고 한다.

> " 로드밸런서의 종류를 나누는 기준은 OSI 7계층을 기준으로 어떻게 부하를 분산하는지에 따라 종류가 나뉩니다. "

<br>

로드밸런서는 3가지의 주요 기능을 통해 로드밸런싱을 진행할 수 있다고 한다. (\* 이 부분은 조금 더 알아봐야할 것 같다.)

**1. Network Addresss Translation (NAT)**

IP 를 변환한다.

**2. Tunneling**

> (인터넷 상에서) 통로를 만들어 통신할 수 있게 해주는 개념이라고 한다.

데이터를 캡슐화하여 연결된 상호 간에만(연결되어진 노드만) 캡슐화된 패킷(데이터)을 해제하여 볼 수 있게한다.

**3. Dynamic Source Routing protocol(DSR)**

(요청에 대한) 응답을 할 때 로드밸런서가 아닌 클라이언트 IP로 응답한다. 즉 응답을 할 때 로드밸런서를 거치지 않고 곧바로 요청을 보낸 client 로 응답한다.

<br><br>

## L2 로드밸런서

MAC Address 를 기준으로 요청을 분산한다. 

Layer 가 낮은만큼 구조가 간단하기에 가격이 저렴하고 성능이 좋다.<br>
Broadcast 패킷에 의해 성능 저하가 발생할 수 있다. 

<br>

## L3 로드밸런서

IP(+ Mac Address) 를 기준으로 요청을 분산한다.


<br>

## L4 로드밸런서

IP, Port를 기준으로 요청을 분산한다.

주로 RoundRobin 알고리즘이 사용된다고 한다.

<br>

## L7 로드밸런서

L7 로드밸런서는 IP, Port + Application(Layer 7) 레벨의 내용(URI, Payload, HTTP Header, Cookie) 등의 내용을 기준으로 요청을 분산한다. 즉 데이터를 보고 판단할 수 있다. (그래서 '콘텐츠 기반 스위칭'이라고 불리우기도 한단다.)

데이터를 보고 판단하는 것과 같이, 더 디테일하게 판단할 수 있다는 것은 그만큼 자원의 소모가 크다는 것을 의미한다고 한다.

> L4 로드밸런서가 단지 부하를 분산시키는 것을 목적으로 한다면, L7 로드밸런서는 조금 더 디테일하게 요청을 분산할 수 있다. (예를 들어, A 기능은 'a' 쪽으로, B 기능은 'b' 쪽으로 등)



<br>

## 참고

1. [L4/L7 로드밸런싱 쉽게 이해하기](https://aws-hyoh.tistory.com/149)
2. [L4/L7 로드밸런서 차이](https://jaehoney.tistory.com/73)