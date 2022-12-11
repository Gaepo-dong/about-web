---
description: TCP 박살내기
---

# TCP

## TCP란?

> TCP 통신이란, 네트워크 통신에서 UDP와 대비되는 신뢰적인 연결방식

즉 unreliable 한 network 상황에서 reliable한 통신을 보장할 수 있는 프로토콜로 OSI 7 layer 기준 transport layer에 속한다.

여기서 reliable을 보장함은 4가지를 지킨다는 뜻이다

* loss : packet이 손실되는것을 방지
* packet order : packet의 순서가 바뀌는것을 방지
* congestion : 네트워크가 혼잡해지지 않게 관리
* overload : receiver가 overload되지 않게 관리

> 이러한 TCP reliable을 보장하기 전 정확하게 전송하기위한 사전준비인 connection을 맺어야한다.&#x20;

연결 방식은 3 way handshake로 구현하고, 해제하는 방식은 4 way handshake를 사용한다.

## 3 way handshake

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p>simple 3 way handshake</p></figcaption></figure>

1. client가 server에게 SYNC(x) 패킷을 보낸다
2. server가 SYNC(x)를 받고, 잘 받아다는 의미로 ACK(x+1)과 SYNC(y)를 보낸다
3. client가 ACK(x+1)을 확인하고 서버가 잘 받았음을 확인하고, 본인도 잘 받았음을 알려주기 위하여 SYNC(y+1)을 전송한다

## 4 way handshake

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption><p>simple 4 way handshake</p></figcaption></figure>

1. client가 server에게 연결을 종료하겠다는 FIN을 전송
2.  서버는 FIN을 받고 확인하였다는 ACK를 전송

    > 여기서 데이터를 마저 보내야 할 수 있으므로 CLOSE\_WAIT 상태를 유지한다


3. client 또한 ACK를 받고, 더 데이터를 받을 수 있으므로 FIN을 기다리는 FIN\_WAIT\_2 상태가 된다
4. server가 모든 데이터를 보내고 FIN 을 전송
5.  클라이언트가 FIN을 받고, 확인했다는 ACK를 전송

    > 클라이언트가 다 받지 못한 데이터가 있을 수 있으므로 바로 끊지 않고, TIME\_WAIT를 통하여 잠시 기다린 후 close한다


6. 서버는 ACK를 받은 후 바로 close
7. TIME\_WAIT이 끝난 후 클라이언트 또한 close

## 흐름제어

> TCP는 이렇게 3-way-handshake 로 연결, 4-way-handshake로 해제를 하지만 이것이 앞서 언급한 reliable한 통신을 만들어 주지는 않는다. network를 reliable할 수 있는 특징 중 loss 와 packet-order를 보장할 수 있는 흐름제어를 살펴보자

흐름제어가 나온 배경은 다음과 같다

* 송신측의 데이터 처리속도가 수신측보다 빠를 때 수신측이 따라잡을 수 없음
* 수신측에서 제한된 저장용량을 초과한 이후에 도착하는 데이터는 손실될 수 있으며, 손실된 상태에서의 요청과 응답은 트래픽을 잡아먹는다

### Stop and Wait

> 매번 전송한 패킷에 대하여 확인 응답을 받고 다음 패킷을 전송하는 방법

패킷을 하나씩 보내야 하는 문제점이 있어 매우 비효율적이다

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption><p>simple stop and wait</p></figcaption></figure>

### Sliding Window

> 수신측에서 설정한 윈도우 만큼 ACK없이 패킷을 전송할 수 있게 하여 데이터 흐름을 동적으로 조절

<figure><img src="../.gitbook/assets/image (1) (4).png" alt=""><figcaption><p>simple sliding window</p></figcaption></figure>

윈도우의 크기는 보통 sender가 receiver에게 ACK를 보낼 때 TCP header에 담아 보낸다.

해당 Sliding Window를 사용하는 동작 방식은 크게 Go back N ARQ 와 Selective Repeat ARQ 로 나뉜다

#### Go back N ARQ

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p>Go-Back-N ARQ flow</p></figcaption></figure>

Go back N 은 단순한 sliding window를 활용한 방식이다. window의 사이즈가 6이면, A 는 \[0, 1, 2, 3, 4, 5] 를 보낼 수 있으므로 천천히 0부터 보내게 된다. 그리고 receiver도 비슷하게 0부터 받으며 천천히 윈도우만큼 받아간다.&#x20;

그러던 중 화면처럼 Data 2에 문제가 생겨 NAK를 받으면 그 후에 오는 데이터는 폐기해 버린다. 그리고 순서대로 올 경우에만 인정을 하고 받게된다.&#x20;

마찬가지로 Data4가 유실된채 Data5가 오게되면, 순서대로 오지 않았기에 폐기하고 Data4를 다시 받기 위하여 NAK4를 보내게 된다.

#### Selective Repeat ARQ

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>Selective Repeat ARQ</p></figcaption></figure>



위 Go-Back-N 을 사용하게되면, 순서는 보장할 수 있지만 잘 받은 데이터도 순서대로 오지 않았을 경우 다시 받아야되는 불필요한 트래픽을 요구하게된다. 그래서 selective-repeat에서는 어떠한 데이터를 받을지 선택하여, 그 데이터부터 전부 다시 받는것이 아닌, 특정한 데이터만 재요청 및 응답이 가능할 수 있게 된다.

그림에서 보이듯이 Data2가 문제가 된 상황에서, 이미 Data3을 받았다면 폐기하지않고, Data2만 재요청 후 받은 후 sort하여 관리하게된다.

마찬가지로 Data5가 유실된 상황에서도, 버퍼로 Data6을 보관하고 NAK 5를 전송한 후 Data5가 다시 올바르게 온다면, sort하여 차례로 패킷을 맞추고 진행하게된다.

## 혼잡제어

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>simple congestion control</p></figcaption></figure>

앞서 TCP의 reliable한 특징 중 congestion 과 overload를 담당해주는 특징이다.

만약 한 라우터에 데이터가 몰리게 되면 어떻게 될까? 또 재전송을 하게 되고 데이터가 몰리게 되는 혼잡을 가중시키는 상황이 올 수 있다. 이러한 혼잡을 송신측에서 전송속도를 줄이며 제어하는 기술이다.

### AIMD(Additive Increase / Multiplicative Decrease)

> 처음 패킷을 보내보고 문제가 없다면 window size를 1씩 늘려가는 방식

패킷 전송에 실패한 경우 보내는 속도를 절반으로 줄이며 조절한다.

시간이 흐를수록 모든 라우터가 평행하게 데이터를 분배받을 수 있다.

하지만, 초기에 높은 대역폭을 가질 수 없으며 혼잡해지는 상황을 미리 감지하지 못하고, 혼잡해진 후 대역폭을 낮추는 방식이다.

### Slow Start

AIMD 방식에서 ACK packet마다 window 사이즈를 1씩 늘려주는 방식으로, 한 주기마다 window size를 2배씩 증가시킬 수 있다.

혼잡 제어현상이 발생하면 window size를 1로 낮춘다.

처음에는 네트워크 수용량을 예측할 수 없지만, 혼잡 현장이 발생한 후 예측을 할 수 있다 -> window size의 절반까지는 2배씩 증가 후 이후부터 1씩 증가하는 방식을 사용

### Fest Retransimit

> packet을 받는쪽에서 다음 packet이 먼저 도착한 경우에도 ACK를 보내는 방식

순서대로 도착한 packet의 다음 packet의 순서를 ACK에 포함하여 보내 손실된다면 송신측에서 순번이 중복된 packet을 받는다. -> 이러한 중복된 순번의 패킷을 3개 받으면 재전송을 하며, 혼잡을 감지하여 window size를 줄여준다.

### Fast Recovery

> 혼잡한 상태가 되면, window size를 1로 줄이지 않고 반으로 줄인 후 선형증가시키는 방식

혼잡상황을 겪고 난 후 AIMD 방식을 사용





참고

{% embed url="https://gyoogle.dev/blog/computer-science/network/%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%20&%20%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4.html" %}
