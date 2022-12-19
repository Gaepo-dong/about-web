---
description: MAC 주소는 무엇일까
---

# MAC

> MAC(Media Access Control) Address

## 개념 바로잡기

Mac Address 란 컴퓨터간 데이터를 전송하기 위해 존재하는 컴퓨터의 물리주소라 할 수 있다.

일반적으로 IP 주소로 컴퓨터끼리 통신을 한다고 알지만, 실제로 그 안에 IP를 MAC Address로 소통하는 부분을 가지고 있고, 이를 ARP라 부른다.

{% content-ref url="arp.md" %}
[arp.md](arp.md)
{% endcontent-ref %}

> 즉 IP 주소간의 통신을 각 라우터의 hop에서 일어나는 MAC 과 MAC 사이의 통신이라 할 수 있다

MAC 주소는 Hardware 주소라고 부르는데, 그 이유는 실제로 NIC(랜카드)에 있는 고유한 번호이기 때문이다.

즉 카드에 주소가 정해져있고, 사용자가 동적으로 변환하기가 쉽지 않다. (ROM에 부여되어 있음)

## MAC 구조

MAC 구조는 48bit로 지정되며, 컴퓨터의 하드웨어에 존재하며 유일한 주소이다.

<figure><img src="../.gitbook/assets/스크린샷 2022-12-19 오후 4.02.08.png" alt=""><figcaption><p>Actual MAC Address</p></figcaption></figure>

### 표기법

표기법은 크게 `:` , `'` , `.` 이렇게 3개로 나뉠 수 있다.

* `:` bc:d0:74:00:00:00
* `'` bc-d0-74-00-00-00
* `.` bcd0.7400.0000

### 구성

맥 주소는 시리얼 넘버의 개념으로 앞 3자리는 OUI의 주소, 뒤 3자리는 호스트 주소로 나뉘게 된다

> 00:00:00:00:00:00\
> \[OUI주소]:\[HOST주소]

그리고 해당 OUI 주소를 특정하여 장비정보를 수집할 수 있다. ex) 삼성, 안드로이드, 맥 ...

실제로 dnschekcr.org 를 사용하여 나의 MAC 주소를 가지고 해당 NIC 를 제작한 회사 (Apple, Inc.)를 확인할 수 있다.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>find info via MAC address</p></figcaption></figure>

참고

{% embed url="https://en.wikipedia.org/wiki/MAC_address" %}
