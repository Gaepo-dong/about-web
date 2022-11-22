---
description: 로드밸런싱에 대하여 알아보자
---

# Load Balancing

> 로드밸런싱이란 많은 트래픽을 여러 대의 서버로 분산해주어 트래픽이 몰리는 상황을 방지하는 것

### Scale-up vs Scale-out

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption><p>scale-up vs scale-out</p></figcaption></figure>

scale-up : 서버 자체의 성능을 확장

scale-out : 서버와 동일하거나 낮은 성능의 서버를 증설하여 운영

{% hint style="info" %}
scale-out 방식으로 서버를 증설한다면 로드밸런싱이 반드시 필요하게 된다!
{% endhint %}

### Load Balancing Algorithm

#### Round Robin (라운드로빈)

서버에 들어온 순서대로 배정하는 방식으로, 모든 서버가 동일한 스펙이고, 연결이 오래 유지되지 않는 환경에서 적합하다.

#### Weighted Round Robin (가중 라운드로빈)

각 서버마다 가중치를 매기고, 가중치가 높은 서버에 클라이언트 요청을 배분하는 방식이다. 서버의 트래픽 처리 능력이 상이한 경우 적합하다.

#### IP Hash (IP 해시)

클라이언트의 IP주소를 특정 서버에 매핑하여 처리하는 방식으로, 사용자가 항상 동일한 서버로 연결되는 것을 보장한다.

#### Least Connection Method (최소 연결)

요청이 들어온 시점에서, 가장 적은 연결상태의 서버에 우선적으로 트래픽을 배분한다. 세션이 길어지거나, 서버에 분배된 트래픽이 일정하지 않은 경우 적합하다.

#### Least Response Time (최소 리스폰)

서버의 현재 연결 상태와 응답시간을 고려하여 배분하는 방식으로, 가장 적은 연결 상태와 짧은 응답시간을 보이는 서버에 우선적으로 서버를 할당시켜주는 방식이다.

### L4 vs L7 vs GSLB

#### L4 Load Balancing

> TCP/UDP 포트 정보를 바탕으로 Load Balancing으로 NLB(Network Load Balancing)이라 부른다

**장점**

* 데이터 안을 살펴보지 않고, 패킷 레벨에서만 분산하기에 속도가 빠르고 효율이 높다
* 데이터의 내용을 복호화하지 않아도 되어 안전하다
* L7에 비해 저렴하다

**단점**

* 패킷의 내용을 살펴볼 수 없어 섬세한 라우팅이 불가능하다
* 사용자의 IP가 바뀌는 상황이라면 연속적인 서비스를 제공하기 어렵다

#### L7 Load Balancing

> TCP/UDP및 HTTP의 URI, FTP의 파일명, 쿠키 정보등을 바탕으로 Load Balancing으로 ALB(Application Load Balancing)이라 부른다

**장점**

* 상위 계층에서의 로드 분산이 가능하여, 섬세한 라우팅이 가능하다
* 캐싱을 활용할 수 있다
* 비정상적인 트래픽을 사전에 필터링 할 수 있어 안정성이 높다

**단점**

* 패킷의 내용을 복호화 해야되기에 높은 비용이 든다
* 클라이언트가 로드밸런서와 인증서를 공유하여, 로드밸런서를 통하여 클라이언트에 접근하는 보안상의 위험이 존재한다

#### FWLB (Firewall Load Balancing)

> 두대 이상의 방화벽에 대해 부하 분산 알고리즘을 적용하는 방식

L4 Switch의 로드밸런싱 기능을 통해 방화벽 간 FWLB를 사용한다. 가용성 및 성능을 향상하고, 동적인 분산을 통하여 응답속도 향상 및 시스템의 변경 없이 방화벽의 확장 및 관리가 가능해진다.

{% hint style="info" %}
한 세션은 한 방화벽만 거쳐야 하기에, 세션 동기화를 위하여 Inbound L4/ Outbound L4 같이 구성해야 된다
{% endhint %}

#### GSLB (Global Server Load Balancing)

DNS는 일반적으로 Round Robin으로 동작하며 단점이 존재한다.

**단점**

* 서버의 상태를 알 수 없어 서비스를 실패하는 유저가 생길 수 있다
* Round Robin임으로 정교한 로드 밸런싱이 힘들다
* 멀리 떨어진 위치의 서버로 연결 될 수 있다

이를 해결하기 위하여 GSLB를 활용 할 수 있다.

**주요기술**

* HealthCheck
  * 주기적으로 서버들의 Health Check를 하는 알고리즘을 가지고 있으며, 상태가 끊기지 않은 서버에게 서비스 해준다
* TTL(Time to Live)
  * DNS에 권한을 가진 네임서버는 특정 레코드에 대하여 TTL을 설정하며, 네임 서버가 TTL동안 캐시에 저장해 둔다. 그리고 요청이 오면 캐싱 된 정보를 반환한다.
  * TTL이 너무 길면, GSLB의 동기화가 제대로 이루어지지 않고, 너무 짧으면 네임서버의 부담이 커지므로, 잘 설정해야된다.
* Distance & Location
  * 주기적으로 거리를 측정하고, DNS Query가 날아오면, 지리적으로 가까운 서버를 반환한다.

더 자세한 내용은 DNS를 참고해보자

{% content-ref url="dns.md" %}
[dns.md](dns.md)
{% endcontent-ref %}



참고

{% embed url="https://www.loadbalancer.org/blog/comparing-layer-4-layer-7-and-glsb-load-balancing-techniques/" %}

{% embed url="https://haeunyah.tistory.com/115" %}

{% embed url="https://tecoble.techcourse.co.kr/post/2021-11-07-load-balancing/" %}

