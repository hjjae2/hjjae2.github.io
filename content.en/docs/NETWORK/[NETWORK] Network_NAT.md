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

NAT(Network Address Translation)는 IP, PORT 정보를 변경하는 기술이다. 패킷(IP, Port)에 변화가 생기기에 패킷에 대한 체크섬도 재계산되어야 한다고 한다.

NAT 를 이용하면 내부의 여러 사설 IP가 외부로 나갈 때 하나의 공인 IP 주소를 통해 나갈 수 있다. 

**'NAT를 사용하여 얻을 수 있는 이점' == '사설 네트워크(사설 IP)를 사용하여 얻을 수 있는 이점'**

<br>

**1. IP 주소 절약**

'사설 IP 사용'의 이점인 것 같다. NAT를 통해 사설 IP, 공인 IP 를 활용할 수 있는데, 사설 IP 를 활용할 수 있다는 것은 그만큼 IP 주소를 절약할 수 있다는 것이다.

**2. 보안**

이것도 '사설 IP 사용'을 통한 이점인 것 같다. 사설 IP 정보에 대해 외부에 노출되지 않는다.