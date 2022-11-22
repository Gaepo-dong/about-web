---
description: RESTful하다는 것
---

# REST

## REST

* **Re**presentational **S**tate **T**ransfer
* 로이 필딩이 말한 소프트웨어 아키텍쳐의 한 형식
* HTTP를 장점을 최대한 활용하기 위한 아키텍쳐
* **자원의 표현**에 의한 **상태 전달**
  * 자원의 표현: 자원을 표기하기 위한 이름
  * 상태 전달: 데이터가 요청되는 시점에서 자원의 상태(정보) 전달

## REST의 원칙

1.  Client-Server 구조

    클라이언트, 서버, 자원, 그리고 요청으로 구성되어 HTTP로 통신하는 클라이언트-서버 구조여야 한다.
2.  균일한 인터페이스

    정보가 규정된 형식 내에서 전송되어야 한다. 이는 다음을 요구한다.

    1. 자원 요청은 식별 가능해야 한다.
    2. 클라이언트가 받은 자원의 표현으로 리소스를 조작 가능해야 한다.
    3. 클라이언트가 자원을 어떻게 처리해야 하는지에 대한 자기서술적 메시지가 있어야 한다.
    4. 애플리케이션의 상태에 대한 엔진으로서 하이퍼미디어. 쉽게 말해 HTML처럼 하이퍼링크가 추가되어서 다음에 어떤 API를 호출해야 하는지를 해당 링크를 통해서 받을 수 있어야 한다.
3.  무상태

    어떠한 클라이언트 정보도 get 요청에서 저장돼서는 안되며 각 요청은 분리되어있고 연결되지 않아야 한다.
4.  계층화 시스템

    클라이언트는 대상 서버에 직접 연결되었는지, 또는 중간 서버를 통해 연결되었는지 알 수 없다.&#x20;
5.  캐시 가능성

    클라이언트는 응답을 캐싱할 수 있어야 한다.
6.  온디맨드 코드(Optional)

    서버에서 클라이언트로 실행 가능한 코드를 전송할 수 있어야 한다. 이 코드는 요청 시에만 실행되어야 한다.

## REST API란?

* REST 아키텍쳐 스타일의 디자인 원칙을 준수하는 API
* HTTP API로 다른 5가지는 잘 지켜지지만, 균일한 인터페이스 부분은 잘 지켜지지 않는다.
* [HTTP API와 REST API의 차이(김영한 님 의견)](https://www.inflearn.com/questions/126743)

## REST 원칙을 지키기 어려운 이유

### Uniform Interface

1. Resource-Based
2. Manipluation Of Resources Through Representations
3. **Self-Descriptive Message**
4. **H**ypermedia **A**s **T**he **E**ngine **o**f **A**pplication **S**tate(HATEOAS)

&#x20;1번과 2번을 합치면 **URI로 지정한 리소스를 Http Method(GET, POST, DELETE, PUT, PATCH 등)를 통해서 표현하고 구분한다!** 라고 할 수 있다**.** 대부분의 REST API라고 부르는 API들이 잘 지키는 원칙이다.

&#x20;진짜 문제가 되는 3번과 4번을 알아보자.

### Self-descriptive Message

* 메시지는 스스로를 설명해야 한다.

#### Example #1

```
GET / HTTP/1.1
```

&#x20;이 메시지는 스스로를 설명하지 못한다. 목적지가 없기 때문이다.

```
GET / HTTP/1.1
Host: www.example.org
```

목적지가 추가되면 스스로를 설명할 수 있게 된다.

#### Example #2

```
HTTP/1.1 200 OK
[ { "op": "remove", "path": "/a/b/c"} ]
```

이 response는 스스로를 설명할 수 없다. 반환하는 데이터 타입이 무엇인지 알 수 없기 때문에, client에서 바로 반환값을 해석할 수 없다.

```
HTTP/1.1 200 OK
Content-Type: application/json
[ { "op": "remove", "path": "/a/b/c"} ]
```

그렇다면 Content-Type을 정의하면 괜찮을까?&#x20;

아니다. json 형식이라는 것은 알았지만, op, path가 무엇을 의미하는 지 알 수 없다.

```
HTTP/1.1 200 OK
Content-Type: application/json-patch+json
[ { "op": "remove", "path": "/a/b/c"} ]
```

json-patch + json형식으로 지정해주면 비로소 자기 서술적 메시지가 된다.

application/json-patch+json은 다음 문서를 참고하자.

{% embed url="https://www.rfc-editor.org/rfc/rfc6902.html" %}

### HATEOAS

* **애플리케이션 상태는 Hyperlink를 이용해서 전이가 되어야한다**

하이퍼링크 하면 떠오르는 HTML은 당연하게 HATEOAS를 만족한다.

```
HTTP/1.1 200 OK
Content-Type: text/html

<html>
<head></head>
<body>
    <a href="/test"></a>
</body>
<html>
```

문제는 JSON형식의 response에서 발생한다.

```
HTTP/1.1 200 OK
Content-Type: application/json
{"id":1,"name":"잠실역"}
```

일반적으로 볼 수 있는 json response이다. 다음 접근 가능 목록을 알 수 없다.

그렇다면 어떻게 표현할 수 있을까?

Link 헤더를 이용하면 가능하다.

```
HTTP/1.1 200 OK
Content-Type: application/json
Link:</subways/1/times>; rel="times",
     </subways/1/detail>; rel="detail"

{"id":1,"name":"잠실역"}
```

Link 헤더 안에 접근 가능한 api 주소를 넣어 HATEOAS를 만족한 모습이다.

## Uniform Interface가 필요한 이유

### 독립적 진화

* 서버와 클라이언트가 각각 독립적으로 진화한다.
* 서버의 기능이 변경되어도 클라이언트를 업데이트할 필요가 없다.
* REST를 만들게 된 계기: How do I improve HTTP without breaking the Web

## 결론

많은 사람들이 REST API라고 말하는 것들은 엄밀하게는 REST API가 아닌 HTTP API이다.

실제로 REST 원칙을 모두 지키기란 매우 복잡하고, 공부하다보니 굳이 그정도까지 지켜야 하나 싶기도 하다.

앞으로 REST API를 만들어봤냐는 질문에 자신있게 "네" 라고 답할 수는 없을 것 같다.









### Reference

{% embed url="https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm" %}

{% embed url="https://velog.io/@nyong_i/%EB%A1%9C%EC%9D%B4-%ED%95%84%EB%94%A9%EC%9D%B4-%EC%9D%B4-%EA%B8%80%EC%9D%84-%EC%A2%8B%EC%95%84%ED%95%A9%EB%8B%88%EB%8B%A4" %}

{% embed url="https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-design" %}

[그런 REST API 로 괜찮은가](https://www.youtube.com/watch?v=RP\_f5dMoHFc)
