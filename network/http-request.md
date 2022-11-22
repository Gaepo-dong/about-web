---
description: HTTP Request와 Response에 대해서 알아보자
---

# HTTP Request & Response

HTTP가 기억이 안난다면?

{% content-ref url="http.md" %}
[http.md](http.md)
{% endcontent-ref %}

### HTTP Request

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>HTTP Request Format</p></figcaption></figure>

#### Request Line

* HTTP Method
  * GET, PUT, POST, HEAD, OPTIONS 등
* URL, Protocol, Port, Domain
  * origin 형식 : 끝에 `?` 와 쿼리 문자열이 붙는 형식으로, GET, POST, HEAD, OPTIONS와 함께 사용
  * absolute 형식 :  완전한 URL 형식으로, GET과 사용
  * authority 형식 : 도메인 이름 및 옵션 포트로 이루어져, HTTP 터널을 구축할 때 CONNECT와 사용
  * asterisk : OPTIONS와 `*` 하나로 서버 전체를 나타낼 때 사용
* HTTP Version

#### Request Header

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>HTTP Request Header</p></figcaption></figure>

* Host : Client가 요청한 도메인의 정보
* User-Agent : 사용자 브라우저에 대한 정보
* Accept : 웹 서버로 수신되는 데이터 중 브라우저가 처리할 수 있는 형식
* Content-Type : Request Message Body의 종류
* Content-Length : Request Message Body의 크기

#### Blank Line

Header와 Body를 구분짓는 공백

#### Request Body

Header에서 지정한 Content-Type 과 Length에 맞춰진 실제 Entity의 정보

### HTTP Response

<figure><img src="../.gitbook/assets/image (5) (2).png" alt=""><figcaption><p>HTTP Response Message Format</p></figcaption></figure>

#### Status Line

* HTTP Version
* Status code
  * 1XX : 정보 전송 임시 응답
  *    2XX : 성공
  * 3XX : 리다이렉션
  * 4XX : 클라이언트 요청 에러
  * 5XX : 서버 오류

#### Response Headers

* Last-Modified : 캐시X된 버전
* ETag : Entity Tag

#### Blank Line

마찬가지로 Header와 Body를 구분짓는 공백

#### Response Message Body

마찬가지로 Header에서 지정한 Content-Type과 Content-Length의 Entity



참고

{% embed url="https://developer.mozilla.org/ko/docs/Web/HTTP/Messages" %}

