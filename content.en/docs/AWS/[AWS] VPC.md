> https://www.44bits.io/ko/post/understanding_aws_vpc 내용입니다.

> *" Amazon Virtual Private Cloud(VPC)를 사용하면 AWS 클라우드에서 **논리적으로 격리된 공간을 프로비저닝**하여 고객이 정의하는 가상 네트워크에서 AWS 리소스를 시작할 수 있습니다. IP 주소 범위 선택, 서브넷 생성, 라우팅 테이블 및 네트워크 게이트웨이 구성 등 가상 네트워킹 환경을 완벽하게 제어할 수 있습니다. VPC에서 IPv4와 IPv6를 모두 사용하여 리소스와 애플리케이션에 안전하고 쉽게 액세스할 수 있습니다. – 아마존 버추얼 프라이빗 클라우드(Amazon Virtual Private Cloud) "*

AWS에서는 AWS계정을 생성할 때 리전 별로 기본 VPC를 함께 생성해준다. 즉, 내 계정의 기본 VPC이다. 리소스를 생성할 때 VPC를 설정하지 않으면 이 기본 VPC가 설정된다.

![](../images/[AWS]%20VPC_34.png)
> 출처 : https://www.44bits.io/ko/post/understanding_aws_vpc

<br>

> *" 이 화면의 중간 쯤에 네트워크(Network)라는 항목이 보입니다. **바로 이 네트워크 속성이 EC2 인스턴스를 실행할 VPC를 선택하는 항목입니다.** VPC를 지정하지 않는 방법은 없습니다. 반드시 인스턴스가 속할 하나의 VPC를 지정해야만 합니다. **서브넷은 VPC에 속해있는 리소스이며, 하나의 인스턴스는 반드시 하나의 서브넷에 속해야합니다.** 단, 기본값을 사용한다면 기본 VPC에서 인스턴스가 생성될 것입니다. "*

즉, AWS 리소스에는 VPC, 서브넷 설정이 반드시 필요하다.

<br>

> *" **VPC 그 자체가 리소스의 이름입니다.** **하지만 동시에 버추얼 프라이빗 클라우드(VPC)를 구축하기 위한 일체의 리소스를 제공하는 서비스의 이름이기도 합니다.** 웹 콘솔 메뉴에서 VPC를 찾아보면 VPC 대시보드에 접속할 수 있습니다. "*

VPC도 SaaS와 같은 리소스/서비스이다.

<br>

## VPC 구성 요소

계정을 처음 만들었을 때 리전 별로 (해당 계정을 위한)VPC가 생성된다고 했다. 이때 만들어지는 리소스 항목은 다음과 같다.

- 1 VPC
- n 서브넷 (Subnet, n은 사용할 수 있는 가용존의 개수)
- 1 라우팅 테이블 (Route Table)
- 1 네트워크 ACL (Network ACL)
- 1 시큐리티 그룹 (Security Group)
- 1 인터넷 게이트웨이 (Internet Gateway)
- 1 DHCP 옵션 세트 (DHCP options set)

### VPC

논리적인 독립 네트워크를 구성하는 리소스

- 이름, IPv4 CIDR 블록을 필수로 갖는다.