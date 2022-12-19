---
description: DHCP를 학습해보자
---

# DHCP

> Dynamic Host Configuration Protocol로, 네트워크 프로토콜의 일종

### DHCP란?

DHCP는 IP주소 및 기타 통신 매개변수를 네트워크에 연결된 장치에 자동으로 할당해준다.

> 가정용인 경우 라우터가 IP 주소를 장치에 할당하는 DHCP 서버 역할을 하게된다.

즉, DHCP가 없었다면 수동으로 네트워크 관리자가 IP 주소를 할당해야되며, 비효율적이며 오류 발생 가능성이 높아진다.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>Simple DHCP Clients &#x26; Server</p></figcaption></figure>

### 동작원리

우선 장치가 네트워크 연결 시 IP 주소를 요청하게 된다. 해당 요청이 DHCP 서버로 전달되어, 서버가 주소를 할당하며, 주소의 이용을 모니터링하고 있다 연결이 해제되면 주소를 다시 가져올 수 있다.&#x20;

이렇게 회수된 IP주소는 다른 장치에 재할당 할 수 있으며, 해당 IP 주소를 활용하여 내부 및 외부 네트워크와 연결을 도와준다.

#### DHCP Flow

DHCP의 과정을 표로 보면 다음과 같다

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>Simple DHCP Flow</p></figcaption></figure>

1.  DHCP Discover

    DHCP 서버를 찾기 위해 Discover message를 ethernet 에 broadcasting 한다

    > 동일 서브넷에 모든 단말들이 메세지를 수신할 수 있음에 주의


2.  DHCP Offer

    Server가 자신의 IP 주소와, PC에게 할당할 수 있는 IP 주소인 1.1.10을 알려준다

    > 마찬가지로, Offer message를 broadcast하여, 동일 서브넷 안의 단말들이 메세지를 수신할 수 있음


3.  DHCP Request

    Client가 확인 후 1.1.1.254인 DHCP 서버에게 1.1.1.10을 할당해달라고 다시 broadcast 하여 전달한다
4.  DHCP ACK

    Client 에게 IP 주소 1.1.1.10을 할당시켜주고, 임대기간(Lease Time)을 1시간으로 전달한다.\
    이 때, Server Identifier에 기록된 IP 주소가 자신인지 확인 후 IP, Subnet, Gateway, DNS 등을 함께 전달해준다

Lease Time이 되어가면, DHCP 서버에게 IP 주소 임대기간을 요청할 수 있는데, 그건 DHCP Request -> DHCP ACK로 이루어지며, 이번에는 서로 IP/MAC 주소를 알기에 unicast 하여 보낼 수 있다.

마찬가지로 PC를 로그오프 하거나 release 할 때 반납을 할 수 있어야 된다. 이때는, ACK없이 DHCP Release 한번에 끝낼 수 있다.

#### DHCP가 설정할 수 있는 Option

* 현지 네트워크와 인터넷 사이 데이터를 라우팅하는 기본 게이트웨이
* IP 주소의 호스트 주소와 네트워크 주소를 분리하는 서브넷 마스크
* DNS서버

#### DHCP 의 IP 할당방식

* Dynamic Allocation
  * 관리자가 DHCP IP 주소를 미리 보유해놓은 경우 사용할 수 있음
  * DHCP 클라이언트가 네트워크 초기화 단계에서 서버에게 IP를 요청
* Automatic Allocation
  * 관리자가 정한 규칙에 따라 IP 주소를 클라이언트에게 영구 할당
  * DHCP 서버에 이전 IP 주소 할당 데이터가 있으며, 동일한 IP 주소를 동일한 클라이언트에게 재할당 할 수 있음
* Manual Allocation
  * 관리자가 각 클라이언트별 고유한 식별자를 IP 주소에 수동으로 할당

### 장점

* 신뢰성 높은 DHCP/IP 주소의 구성
  * 동일한 IP 주소를 이용하는 사용자 간 충돌을 방지
* 높은 이동성
  * 네트워크 밤위 내에서 이동이 자유로움
* 효율적인 네트워크 관리
  * 별도의 IP 할당 서버가 필요하지 않음
* IP 체계의 유연성
  * 최종 사용자에게 지장을 주지 않고, IP 주소 체계 변경이 가능

### 문제

* 승인받지 않은 DHCP서버가 잘못도니 정보를 제공할 수 있음
* 승인받지 않은 DHCP 서버를 가로채 리소스에 접근할 수 있음





참고

{% embed url="https://nordvpn.com/ko/blog/what-is-dhcp/" %}
