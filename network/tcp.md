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

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>simple 4 way handshake</p></figcaption></figure>



## 흐름제어

## 혼잡제어
