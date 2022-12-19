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

<figure><img src="../.gitbook/assets/image (9) (2).png" alt=""><figcaption><p>modocode.com의 DNS query 순서</p></figcaption></figure>

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

일반적으로 Round Robin 방식을 활용하지만, 이를 활용한 경우 문제가 발생 할 수 있다. 근본적인 문제들은 다음과 같다

* 서버의 상태를 알 수 없어 서비스를 실패하는 유저가 생길 수 있다
* Round Robin임으로 정교한 로드 밸런싱이 힘들다
* 멀리 떨어진 위치의 서버로 연결 될 수 있다

즉 DNS에서 IP에 대한 Health Check와 부하 분산 알고리즘을 적용하면 되고, GSLB과 이를 구현해준다.

{% hint style="info" %}
Health Check의 동작원리는 사용자가 GSLB에서 제공하는 IP 주소로 접근 했을 때, 정상적인 응답이라면 Health OK, 오류값이 온다면 재전송 후 재전송 횟수 이내에 정상적인 응답이 없다면 다운 됐다고 판단&#x20;
{% endhint %}

#### GSLB 동작 방식

<figure><img src="../.gitbook/assets/image (13) (1).png" alt=""><figcaption><p>GSLB Flow</p></figcaption></figure>

이제 GSLB 버전으로 5번부터 확인해보자

&#x20; 5\. `modocode.com`에 대한 권한은 GSLB에 있으므로, GSLB에 물어볼 수 있게 IP 주소를 제공

&#x20; 6\. Local DNS가 GSLB에 `modocode.com`에 대한 Query를 물어본다

&#x20; 7\. 8. 9. 10. 11. GSLB가 `modocode.com`에 대한 헬스 체크를 수행 후, 알고리즘에 따라 최적의 IP 주소를 반환한다

&#x20; 12\. 최종으로 받은 IP 주소로 패킷을 보낼 수 있다

#### GSLB Algorithm

앞서, GSLB가 최적의 IP를 반환해준다고 했는데, 이  알고리즘에는 여러 방식이 존재한다. AWS Route53은 직접 라우팅 정책을 선택할 수 도 있는데, 방식들은 다음과 같다.

<details>

<summary>GSLB Algorithms</summary>

* **단순 라우팅 정책(Simple routing policy)**&#x20;
  * 도메인에 대해 특정 기능을 수행하는 하나의 리소스만 있는 경우   사용

<!---->

* **장애 조치 라우팅 정책(Failover routing policy)**&#x20;
  * 액티브-패시브 장애 조치를 구성하려는 경우에 사용

<!---->

* **지리 위치 라우팅 정책(Geolocation routing policy)**&#x20;
  * 사용자의 위치에 기반하여 트래픽을 라우팅하려는 경우에 사용

<!---->

* **지리 근접 라우팅 정책(Geoproximity routing policy)**&#x20;
  * 리소스의 위치를 기반으로 트래픽을 라우팅하고 필요에 따라 한 위치의 리소스에서 다른 위치의 리소스로 트래픽을 보내려는 경우에 사용

<!---->

* **지연 시간 라우팅 정책**&#x20;
  * 여러 AWS 리전에 리소스가 있고 최상의 지연 시간을 제공하는 리전으로 트래픽을 라우팅하려는 경우에 사용

<!---->

* **IP 기반 라우팅 정책**
  * 사용자의 위치에 기반하여 트래픽을 라우팅하고 트래픽이 시작되는 IP 주소가 있는 경우에 사용

<!---->

* **다중 응답 라우팅 정책(Multivalue answer routing policy)**
  * Route 53이 DNS 쿼리에 무작위로 선택된 최대 8개의 정상 레코드로 응답하게 하려는 경우에 사용

<!---->

* **가중치 기반 라우팅 정책(Weighted routing policy)**
  * 사용자가 지정하는 비율에 따라 여러 리소스로 트래픽을 라우팅하려는 경우에 사용

</details>



참고

{% embed url="https://docs.aws.amazon.com/ko_kr/Route53/latest/DeveloperGuide/routing-policy.html" %}

{% embed url="https://aws.amazon.com/ko/route53/what-is-dns/" %}

{% embed url="https://haeunyah.tistory.com/115" %}
