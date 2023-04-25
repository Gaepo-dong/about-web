---
description: User Datagram Protocol
---

# UDP

## UDP의 개념

UDP는 User Data Protocol의 약자로, 사용자 데이터그램 규약 즉 데이터를 데이터그램 단위로 처리하는 프로토콜이다.

데이터그램이란, 독립적인 관계를 나타내는 패킷으로 경로 없이 매 패킷마다 다른 경로로 전송되며 다음 그림과 같이 독립적인 관계를 지니는 프로토콜이다.

<figure><img src="../.gitbook/assets/image (2) (2).png" alt=""><figcaption><p>Easy UDP Flow</p></figcaption></figure>

### UDP Socket Flow

<figure><img src="../.gitbook/assets/image (6) (3) (1).png" alt=""><figcaption><p>Easy UDP socket flow</p></figcaption></figure>

## UDP의 특징

* 비연결형의 서비스로 데이터그램으로 데이터를 교환
* 송수신에 있어 SYNC/ACK 등의 정보를 전달하지 않음
* CheckSum에서 최소한의 오류만을 검출
* 신뢰성이 낮음
* 속도가 빠름
* 서버 소켓과 클라이언트 소켓의 구분이 없음
* 서버와 클라이언트는 1:1, 1:N, N:M 등으로 연결이 가능
* 흐름제어가 없어 패킷이 제대로 전송 혹은 오류가 있는지 확인할 수 없음

즉 TCP와 비교하였을 때, 신뢰할 수 있는 부분을 제외한 것이라 볼 수 있다. 모든 데이터그램이 독립적으로 처리되고, 패킷에 순서나 재조립, 흐름제어, 혼잡제어를 하지않아 속도가 빠르지만 신뢰성을 보장할 수는 없다. 즉 신뢰성보다 연속성이 중요한 RTC와 같은 분야에서 자주 사용된다.

## TCP vs UDP

| Protocol   | TCP       | UDP               |
| ---------- | --------- | ----------------- |
| Connection | 연결형 서비스   | 비연결형 서비스          |
| 패킷 교환 방식   | 가상 회선 방식  | 데이터그램 방식          |
| 전송 순서      | 전송 순서 보장  | 전송 순서가 바뀔 수 있음    |
| 수신 여부 확인   | 수신 여부를 확인 | 수신 여부를 확인하지 않음    |
| 통신 방식      | 1:1       | 1:1 or 1:N or N:M |
| 신뢰성        | 높음        | 낮음                |
| 속도         | 느림        | 빠름                |



{% embed url="https://mangkyu.tistory.com/15" %}
