---
description: NAT에 대하여 알아보자
---

# NAT

## NAT란

Network Address Translation의 약자로, 네트워크 주소 변환을 담당해준다. 즉 하나의 공인 IP를 여러개의 사설 IP로 변환해주는 시스템이다.&#x20;

## NAT를 사용하는 이유

왜 모든 컴퓨터가 공인 IP를 할당받지 않고 사설 IP로 변환이 필요할까?&#x20;

1.  우선 IPv4 형식에서 IP 주소의 개수가 부족하다.

    IPv4 는 xxx.xxx.xxx.xxx 형태로 표현을 하는데 최대 2^32로 43억개 정도로 전 세계의 IP를 할당하기에는 부족하다
2. 공인 IP가 무지하게 비싸다...

즉 이러한 이유에서 NAT가 하나의 공인 IP인 공유기에서 접속하는 사람들은 사설 IP를 받게 설정할 수 있게 도와준다.

나의 Private IP와 Public IP가 다음과 같을 때 NAT 구조를 보면 다음과 같다.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>My Private IP</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p>My Public IP</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption><p>Simple NAT</p></figcaption></figure>

> Private Network의 대역
>
>
>
> 10.0.0.0 \~ 10.255.255.255
>
> 172.16.0.0 \~ 172.31.255.255
>
> 192.168.0.0 \~ 192.168.255.255

## NAT의 장점

1.  IP 주소 절약

    앞서 말했듯 IPv4 기준 최대 43억개만 사용가능한 상황에서, 한 공인 IP에서 Private Network 의 대역에 있는 주소를 사용가능하므로, 매우 많은 네트워크 연결이 가능해진다.
2.  보안

    NAT는 STUN 혹은 TURN 서버를 두지 않는한 외부에서 사설망 내부로의 접근이 불가능한 구조가 되어 보안적으로 안전해진다.
3.  IP 주소 관리의 편의

    DHCP가 Private IP들을 관리하여 동적으로 host들의 IP들을 관리할 수 있다.

## NAT의 원리

사설망에서 외부망으로 통신을 시도할 때 Gateway를 지나게 되고, 해당 Gateway를 지날 때 내 SRC\_IP를 Public IP로 수정하여 보내게 된다. 하지만, 이렇게 보낼 경우 외부에서 나의 Private IP를 모르기에 통신을 할 수 없게 된다.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>Simple NAT with problem</p></figcaption></figure>

### NAT Table

그래서 NAT Table을 활용하여, Gateway에 나에대한 정보를 미리 보관해둔다. Protocol, Private IP, SRC\_IP, DEST\_IP 에 해당한 테이블을 기록해두고, 응답 패킷이 돌아올 때 그 값을 찾아 원하는 사설망에 보내줄 수 있다.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p>Simple NAT with NAT Table</p></figcaption></figure>

그리하여, NAT의 장점은 보안을 봤을 때, 외부에서 내부로 처음 들어오는 상황이라면 NAT Table에 기록되지 않아 사설망으로 찾을 수 없으니 보안적으로 안전해진다는 말을 이해할 수 있다.

### NAPT

하지만 위에와 같은 NAT Table만 썼을 때, 같은 Private IP에서 같은 프로토콜과 같은 DEST\_IP로 접근했을 때, 어떤 사설망에게 응답을 보내야되는지 찾을 수 없게된다. 그런 경우 NAPT로 해결이 가능하다.

Network Address Port Translation으로, 포트까지 붙여 전달해주어, 발신자의 포트번호로 구별하여 사설망을 잘 찾아갈 수 있게 설정하게 도와준다.

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption><p>Simple NAT with NAPT</p></figcaption></figure>

참고

{% embed url="https://5kyc1ad.tistory.com/254" %}
