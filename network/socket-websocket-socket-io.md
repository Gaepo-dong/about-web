---
description: Socket, WebSocket, SocketIO에 대하여 알아보자
---

# Socket, WebSocket , Socket IO

## Socket

> TCP/IP 4계층에서 동작하는 네트워크에서 두 개의 프로그램 간 양방향 통신

즉 소프트웨어와 소프트웨어간에 연결을 하여 데이터를 송수신 시켜주는 단순한 친구

<figure><img src="../.gitbook/assets/image (1) (2) (1).png" alt=""><figcaption><p>Socket Flow</p></figcaption></figure>

### 클라이언트 흐름

1.  소켓 생성

    : Socket 껍데기 생성으로, TCP는 stream type, UDP는 datagram type으로 준비
2.  연결 요청

    : 서버소켓에 대한 IP 주소와 포트번호로 타겟을 지정 후 connection 연결을 시도

    > 요청을 보내고 끝이 아닌, 응답이 와야 connection이 끝이 난다
3.  데이터 송수신

    : 마찬가지로, 응답이 돌아와야 끝이 나는 형식

    > 송신은 보내고 끝이 나지만, 수신하는 경우 얼마나 보낼지 모르기에 다른 thread로 실행해야된다
4.  소켓 닫기

    : 할거 다 하면 close()

### 서버 흐름

1.  소켓 생성

    : 클라이언트와 동일하지만, 해당 소켓은 클라이언트의 연결을 해주는 소켓으로, 실제 커넥션용 소켓은 아니다
2.  바인딩

    : 서버 소켓이 고유한 포트 번호를 만들 수 있도록 소켓과 포트번호를 결합해 주는 과정
3.  클라이언트의 연결 요청 대기

    : 클라이언트가 요청 연결을 할 때 까지 대기하다 요청이 오면 대기상태를 종료하고 클라이언트에게 알려준다
4.  클라이언트 연결 수락

    : 요청을 수락하며 바인딩에서 했던 소켓을 새로 생성한다
5.  데이터 송수신

    : 클라이언트와 동일
6.  소켓 닫기

    : 클라이언트와 동일

## WebSocket

> HTML 환경에서 Socket을 활용하기 위한 프로토콜

웹소켓이 나온 배경은 다음과 같다

1. Full-Duplex
   1. 데이터의 송수신을 동시에 처리
   2. 클라이언트 및 서버가 원할 때 데이터를 송수신
2. RTC (Real Time Communication)
   1. 연속된 데이터를 실시간으로 처리
3. Header의 무거움
   1. 여러번 소통을 할 때마다 Header를 달고 Request, Response하기에 트래픽이 부담

물론 HTTP로 해당 기술들을 구현할 수는 있다

1. Polling : 클라이언트가 일정한 주기로 요청 송신하지만, 불필요한 connection이 많아짐
2. Long Polling : Request후 조금 더 대기하여 이벤트를 받지만, 응답을 받을 때 까지 연결이 종료되지 않는다
3. Streaming : 서버에 요청을 보내고 끊지 않고 수신하지만, 클라이언트에서 서버로 데이터 전송이 어렵다

즉 단점이 너무 많게되어 HTTP를 대신하는 Socket이 나오게 되었다

### 웹소켓 동작 방법

웹소켓도 그냥 연결할 수 있는것이 아니라, HTTP Request로 WebSocket 연결을 할 것이라고 Handshake를 거친 후 할 수 있다.

WebSocket 연결에 대한 Flow는 다음과 같다.

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption><p>WebSocket Flow</p></figcaption></figure>

> 여기서 Upgrade는 앞으로 서버, 전송, 프로토콜 연결을 다른 프로토콜(웹소켓)로 업그레이드 하겠다는 뜻

> 101 response는 Switching Protocol로 웹소켓으로 연결 됐다는 뜻

### 단점

* 복잡하다
  * HTTP와 달리 Stateful 하기에, 항상 서버와 클라이언트 간의 연결을 유지
* Socket을 유지하는것이 비용
* HTML5, HTTP/1.1 이하에서 지원하지 않는다

## Socket IO

> Socket IO란 이벤트 기반의 서버와 브라우저 간 양방향 통신을 도와주는 라이브러리

{% hint style="info" %}
Socket IO는 WebSocket의 구현이 아님에 주의
{% endhint %}

WebSocket을 사용할 수 있다면 사용하지만, 매 packet에 추가적인 metadata를 사용하는 등 WebSocket이라고 말 하기는 애매하다. 심지어 필요하다면 long-polling 기법도 사용한다.&#x20;

```
// 이건 안됨
const socket = io('ws://echo.websocket.org');
```

### 특징

* HTTP long-polling fallback
  * WebSocket으로 연결이 안되는 환경(HTML4처럼 WebSocket이 없는 경우)이라면, HTTP long-polling 기법을 활용
* Automatic reconnection
  * 주기적으로 연결이 끊겼는지 확인 후, exponential back-off delay로 자동으로 연결이 가능
* Packet buffering
  * 클라이언트 연결이 끊어졌을 때, 패킷이 버퍼링 되어 다시 연결시에 전송
* Acknowledgements
  * emit으로 전송, on으로 응답
* Broadcasting
  * emit으로 전송 시, 모든 연결된 client에게 전송
* Multiplexing
  * namespace로 논리적인 방 구현 가능

### 단점

* long-polling 방식을 사용할 시 WebSocket보다 느려진다
  * `transports: ['websocket']` 으로 강제화 할 수 있음
* 정확성 유무를 보장하지 않는다
  * `ACK` 를 전송하여 잘 전송 되었다는것은 알 수 있지만, 정확한 데이터인지 보장할 수는 없음



참고

{% embed url="https://developer.mozilla.org/ko/docs/Web/API/WebSocket" %}
J
{% endembed %}

{% embed url="https://mingule.tistory.com/m/60" %}

