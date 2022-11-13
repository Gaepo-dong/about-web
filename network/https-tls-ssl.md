---
description: HTTPS, TLS, SSL에 대하여 알아봅시다
---

# HTTPS, TLS, SSL

* [#https](https-tls-ssl.md#https "mention")
* [#http-vs-https](https-tls-ssl.md#http-vs-https "mention")
* [#ssl-and-tls](https-tls-ssl.md#ssl-and-tls "mention")
* [#undefined-1](https-tls-ssl.md#undefined-1 "mention")
* [#tls](https-tls-ssl.md#tls "mention")
* [#undefined-2](https-tls-ssl.md#undefined-2 "mention")
* [#tls-handshake](https-tls-ssl.md#tls-handshake "mention")

### HTTPS란?

> HTTPS(HyperText Transfer Protocol Secure)로 HTTP에서 보안이 강화된 버전이다

{% embed url="https://gaepodong.gitbook.io/about-web/network/http" %}
HTTP 복습하기
{% endembed %}

### HTTP vs HTTPS

* HTTPS는 SSL 인증서를 통한 보안 기능을 추가
* HTTP는 80포트, HTTPS는 443포트를 사용
* HTTPS는 네트워크 상에서 중간에 정보를 볼 수 없음
* HTTPS가 HTTP보다 느리다
  * 하지만 효율이 좋은 HTTP/2.0이 HTTPS 환경에서 돌아가므로 정확한 말은 아님
* HTTPS가 SEO에 더 효율적이다
  * 구글이 HTTPS전환시 SEO에서 가산점을 준다

### SSL & TLS

> SSL과 TLS 모두 네트워크를 통해 작동하는 서버, 시스템 및 응용프로그램간 인증 및 데이터 암호화를 제공하는 암호화 프로토콜

SSL(Secure Socket Layer)이 1995년 Netscape에 의해 공개되고, 표준화되어 현재 계승된 TLS(Transport Layer Socket)로 사용된다.

SSL의 구성은 다음과 같다.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>SSL Architecture</p></figcaption></figure>

### 암호화

TLS의 동작과정을 보기 전, 대칭키 암호화와 비대칭키 암호화에 대한 선행학습을 필요하다.

#### 대칭키 암호화

> 암호화를 하는 키와, 복호화를 하는 키가 동일한 방식

대칭키는 상대방과 서로 공유해야 하기에, 전달하는 과정에서 해킹당할 위험이 있다. 그래서 나온것이 비대칭키 암호화 방식.

#### 비대칭키 암호화

> 한 쌍의 키로, 암호화와 복호화를 진행

A, B 두 개의 키로 A키로 암호화한 데이터는 B키로만 복호화 할 수 있게 설정한다. 마찬가지로 B키로 암호화된 데이터는 A키로 복호화 할 수 있다.

> 공개키와 개인키라고 부름

그리고 이러한 암호화, 복호화 방식은 RSA 알고리즘을 통해 구현된다.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p>간단한 비대칭키 암호화</p></figcaption></figure>

하지만 비대칭키는 RSA 알고리즘을 이용하여 CPU 리소스를 많이 소모하게 된다.

### TLS 암호화 방식

처음에 대칭키를 서로 공유하는 통신을 RSA 비대칭키 방식을 이용하고, 실제 통신을 할 때는 대칭키 방식으로 데이터를 주고 받는다.

즉 안전하게 비대칭키 통신을 이용하여 대칭키를 전달 받은 후, 대칭키를 해킹당할 위험을 방지하고 대칭키로 통신을 한다.



### 인증서

> 서버가 신뢰할 수 있는 서버라는것을 확인하는 작업

인증서에 포함되는 내용은 다음과 같다.

1. 서비스 정보(인증서를 발급한 CA 및 서비스 도메인)
2. 서버측의 공개키
3. 지문, 디지털 서명 등

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption><p>modoco의 인증서</p></figcaption></figure>

우선 인증서를 발급 받는 방법부터 알아보자

#### CA (Certificate Authority)

> 인증서를 발급해주는 기관

CA의 인증과정은 다음과 같다

1. 서비스에서 CA로 도메인과 공개키를 전달하며 인증서 발급 요청을 한다
2. 서비스의 공개키를 SHA-256으로 해싱하여 인증서의 지문으로 등록
3. 등록한 지문을 CA의 개인키로 암호화 하여 인증서의 서명으로 등록
4. 인증서 발급

> CA만의 공개키와 개인키가 있음에 주의

#### 서버 인증

> 클라이언트의 CA리스트를 활용하여 인증

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>keyChain에 등록되어 있는 인증서</p></figcaption></figure>

서버가 CA 기관에서 인증받았다는 무결성을 확인하는것은 다음과 같다.

1. 클라이언트가 서버에 접속
2. 서버가 CA에서 발급받은 인증서를 전달
3. CA의 공개키로 서버 인증서의 서명을 복호화
4. 디지털 서명의 해시값과, 공개키로 해시한 값이 일치하면 위조되지 않았음을 판단
5. 인증 되었으므로 TLS 암호화를 통하여 대칭키를 전달

### TLS Handshake

> HTTPS의 작동 원리의 끝판왕인 TLS Handshake를 알아보자

TLS handshake는 사용자가 HTTPS를 통해 웹 사이트를 탐색을 시작할 때 발생한다.&#x20;

{% hint style="info" %}
TCP handshake가 끝난 후 발생
{% endhint %}

#### TLS handshake의 역할

* TLS version 교환
* 사용할 암호방법 교환
* 서버의 ID 인증
* handshake후 대칭키롤 사용할 세션 키 생성

#### 과정

1. Client hello: Client가 지원하는 TLS version, 지원하는 암호 방식, Client random data를 전송
2. Server hello: SSL 인증서, 서버에서 선택한 암호 방식, Server random data 전송
3. 인증: Client가 인증서 검증 및 Server의 공개키 획득
4. pre master secret: Client가 pre master secret인 random data를 Server의 공개키로 암호화 하여 전송
5. 개인 키 사용: 서버가 pre master secret 해독
6. 세션 키 사용: Client, Server 둘 다 random data 3가지를 통하여 세션 키를 생성
7. 클라이언트 준비 완료: 세션키로 완료 메세지 전송
8. 서버 준비 완료: 세션키로 완료 메세지 전송
9. 대칭키 암호화 성공: handshake 끝!!

그림으로 도식화 하여 보면 다음과 같다

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption><p>full TLS handshake diagram</p></figcaption></figure>



참고

{% embed url="https://smartits.tistory.com/209" %}

{% embed url="https://babbab2.tistory.com/4" %}

{% embed url="https://www.cloudflare.com/ko-kr/learning/ssl/what-happens-in-a-tls-handshake/" %}
