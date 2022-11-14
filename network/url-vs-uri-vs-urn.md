---
description: URL, URI, URN 차이를 알아보자
---

# URL vs URI vs URN



## URI란?

* Uniform Resource Identifier
* 인터넷에 있는 자원을 식별하는 방법
* 하위 개념으로 URL(Locator), URN(Name), URC(Citation) 등이 있다.
* URC(citation)은 자원이 아닌 메타데이터를 가리킬 때 사용

![](<../.gitbook/assets/image (7).png>)

## URL이란?

* Uniform Resource Locator
* 자원이 위치한 정보를 나타냄

### URL의 구성 요소

ex) https://www.google.com:443/search?q=hello\&hl=ko

* scheme(https)
  * 프로토콜
* userinfo(생략, username@)
  * 사용자 정보 포함해 인증
  * 거의 사용하지 않음
* host(www.google.com)
  * 호스트명
  * 도메인명 또는 IP주소
* port(443)
  * 접속 포트
* path(/search)
  * 리소스 경로
* query param(q=hello\&hl=ko)
  * key=value형태로 쿼리 파라미터 전달

## URN이란?

* Uniform Resource Name
* 이름으로 자원을 식별
* 실제 자원을 찾기 위해서는 URN을 URL로 변환하여 이용
* URL의 한계로 인해 착수되었다
  * URL은 주소이지, 실제 이름이 아니다.
  * 리소스가 옮겨지면 해당 URL을 더는 사용할 수 없다.
  * 객체의 위치와 상관없이 그 객체를 가리키는 실제 객체의 이름을 사용하여 해결하는 방법이 URN

### URN 문법

urn:isbn:0451450523

urn:uuid:6e8bc430-9c3a-11d9-9669-0800200c9a66

* isbn이나 uuid를 해석가능한 프로그램이 있어야 동작함

### AWS의 ARN

*   arn:aws:kms:ap-northeast-2:888869365995:key/679f76c9-a93d-4144-8163-9319e9d9cd18


* AWS는 URN을 확장한 ARN을 사용
* arn 문법이고, aws 클라우드의, kms 서비스이고, ap-northeast-2(서울) 지역의, 888869365995 사용자의, key/679f76c9-a93d-4144-8163-9319e9d9cd18 리소스를 말함

## URL과 URN의 차이

* URL은 리소스를 어떻게 얻을 것이고 어디에서 가져와야 하는지 명시하는 URI
* URN은 리소스 자체를 특정하는 것을 목표로 하는 URI

