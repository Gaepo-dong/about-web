---
description: MAC 다음은 ARP
---

# ARP

> Address Resolution Protocol

앞서 MAC이 어떤 역할을 하는지 알아보았는데, IP주소를 MAC과 매칭 시켜주는 프로토콜인 ARP도 알아보자.

{% content-ref url="mac.md" %}
[mac.md](mac.md)
{% endcontent-ref %}

## ARP

왜 ARP를 사용해야되는지 알아보려면 MAC 주소를 사용하는 이유와 동일하다 할 수 있다. IP 주소는 네트워크상에 존재하는 주소이기에, 정확한 end point의 주소를 알 수 없다.&#x20;

IP 주소와 MAC 주소를 1:1 대응하여 테이블로 정리하는 것을 `ARP Table` 이라 부른다.

그리고 스위치/라우터에 어느 Port/PC 에서 MAC 주소가 연결되어 있는지 `MAC 주소 Table` 로 확인할 수 있다.

### IP 주소만 가지고 통신을 하는 경우

IP 주소 자체가 가변적이고, 한 IP 안에 여러 서버 혹은 PC가 존재할 경우 특정하기 쉽지 않다.

### MAC 주소만 가지고 통신을 하는 경우

한 라우터에 모든 통신을 할 수 있는 PC / 서버의 MAC 주소를 다 등록한다면 이론적으로 가능하겠지만, 이런 경우 라우팅 알고리즘을 적용할 수 없고, 모든 MAC 주소를 저장하기 현실적으로 어렵다.

## 동작과정

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>simple ARP flow #1</p></figcaption></figure>

1.  PC 1이 ARP 요청을 Broadcast 하여 PC2의 IP 주소를 가진 잔말이 있는지 물어본다.

    > PC2의 MAC Address를 모르기에 기기본 게이트웨이 FF:FF:FF:FF:FF:FF 로 broadcast 로 전송


2. 네트워크 스위치에 PC1의 MAC 주소가 저장되어 있지 않은 경우, A의 MAC 주소를 자신의 MAC Table에 저장한다.
3. 네트워크 장비의 MAC Table에 PC2의 IP 주소에 대항하는 MAC주소가 없다면, 다시 broadcast 하는 ARP request를 보낸다.
4. PC2가 자신의 MAC 주소를 알려주는 ARP Reply를 보내게 된다.
5. 네트워크 장비에서 B에 해당하는 MAC 주소와IP 주소를 매핑하여 저장한다.
6. PC1에게 PC2에 해당하는 MAC주소를 전달해주며 끝!

{% hint style="info" %}
ARP Request는 broadcast, ARP Reply는 unicast임에 주의!
{% endhint %}

참고

{% embed url="http://www.ktword.co.kr/test/view/view.php?m_temp1=10" %}

{% embed url="https://www.stevenjlee.net/2020/06/07/%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-arp-address-resolution-protocol-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C/" %}
