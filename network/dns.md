---
description: DNS의 동작원리에 대하여 알아보자
---

# DNS

### DNS란?

> Domain Name System으로, 호스트의 도메인 이름을 네트워크주소 즉 IP로 변환해준다

naver의 IP를 nslookup 명령어로 찾아보면 다음과 같다.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption><p>naver.com의 domain 주소</p></figcaption></figure>

그리고 해당 IP 4개 다 naver.com 으로 잘 매핑되는 것을 볼 수 있다.

### 도메인 레코드

#### A 레코드

> A 레코드는 일반적인 DNS의 내용으로, Domain 주소로 매핑된 IP주소를 알려줌

#### CNAME

> Canonical Name으로, 도메인 주소를 또 다른 도메인 주소로 매핑이 가능&#x20;

이를 활용하여 modocode.com 과 모도코.com 모두 같은 IP를 가리킬 수 있게 설정 할 수 있다.

#### MX 레코드

> Mail eXchange 레코드로, 메일을 주고 받을 수 있게 도와주는 서비스

예를들어, yeonggi@kakao.com 에서 kakao.com 의 도메인이 메일을 주고받을 수 있는 서버를 알려주기에 소통할 수 있다.

#### TXT 레코드

> 일반적인 텍스트 내용으로, 사람이 서버에 대한 성격, 이름, 자원등을 쉽게 string 형식으로 파악할 수 있게 도와줌

### DNS의 서비스 유형

#### 신뢰할 수 있는 DNS

* 도메인에 대한 최종 권한이 존재
* 재귀적 DNS 서버에 IP 주소가 담긴 답을 제공
* DNS 쿼리에 응답하면 IP 주소로 변환

#### 재귀적 DNS

* 클라이언트가 신뢰할 수 있는 DNS서비스에 쿼리를 수행하지 않음
* reosolver 또는 재귀적 DNS 서비스가 대신 연결
* DNS 레코드를 소유하고 있지 않지만, 사용자를 대신하여 DNS 정보를 가져올 수 있게 중간자 역할을 제공
* 캐시된 레퍼런스가 있는 경우, IP 정보를 바로 제공&#x20;

### DNS 작동원리

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>modocode.com의 DNS query 순서</p></figcaption></figure>

순서를 보면 다음과 같다

1. PC브라우저가 `https://modocode.com`을 입력하고, Local DNS 에게 해당 도메인에 대한 IP 주소를 물어본다
2. Local DNS에서 캐싱된 Domain 정보가 있다면, 바로 제공해주지만, 없을경우 Root DNS에게 `https://modocode.com`에 대한 DNS Query를 물어본다
3. Root DNS서버는 `https://modocode.com` 에 대한 IP 주소가 없으니, com DNS 즉 TLD(Top Level Domain)에 다시 물어보라며 com DNS의 서버 정보를 알려준다
4. 받은 TLD의 주소로 `https://modocode.com`에 대한 DNS query 를 날려본다
5. `modocode.com` 을 관리하는 DNS 정보를 전달해준다 (여기서 modocode는 AWS Route53으로 제작되어, Route53의 DNS Server를 전달)
6. 받은 Route53 DNS 주소로 `https://modocode.com`에 대한 DNS query 를 날려본다
7. Local DNS 서버에게 `https://modocode.com`에 해당하는 `54.230.61.5` 이라는 IP주소를 알려준다
8. Local DNS에 `https://modocode.com`와 `54.230.61.5`  를 매핑 후 브라우저에게 IP주소를 알려준다

### GSLB 활용

> DNS의 고질적인 문제인 Round Robin을 해결해보자

앞에서 naver.com에 해당하는 IP를 봤을 때, 한 IP가 아닌 여러 IP가 제공된다는것을 볼 수 있었다. 그럼 브라우저는 어떤 IP로 접속을 해야될까?

일반적으로 Round Robin 방식을 활용하지만, 이를 활용한 경우 문제가 발생 할 수 있다.





참고

{% embed url="https://aws.amazon.com/ko/route53/what-is-dns/" %}

{% embed url="https://haeunyah.tistory.com/115" %}
