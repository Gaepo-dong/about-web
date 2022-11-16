---
description: Socket, WebSocket, SocketIO에 대하여 알아보자
---

# Socket

## Socket

> TCP/IP 4계층에서 동작하는 네트워크에서 두 개의 프로그램 간 양방향 통신

즉 소프트웨어와 소프트웨어간에 연결을 하여 데이터를 송수신 시켜주는 단순한 친구

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>Socket Flow</p></figcaption></figure>

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



## Socket IO



참고

{% embed url="https://developer.mozilla.org/ko/docs/Web/API/WebSocket" %}
J
{% endembed %}

{% embed url="https://mingule.tistory.com/m/60" %}

