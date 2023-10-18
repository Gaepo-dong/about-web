---
description: 혼잡 제어 디테일하게 분석하기
---

# Congestion Control

앞서 TCP에서 간단하게 혼잡제어에 대해서 알아보았는데, 더 자세하게 공부할 필요가 느껴 다시 정리를 해보자



### 혼잡제어의 탄생배경

혼잡제어란 네트워크의 불안정한 상황들을 해결하기 위하여 데이터 전송을 제어하는것이다. 여기서 말하는 불안정한 상황들은 크게 두가지로 나뉜다.

1. 같은 ACK가 연속적으로 오는 경우 -> 송신측이 연속적으로 ACK를 받는 경우 어떠한 이유로 특정 sequence 이후의 데이터를 제대로 처리하지 못하고 있음
2. Timeout -> 송신측이 보낸 후 일정 기간동안 ACK를 받지 못한 경우, packet을 잃어버리거나 문제가 생겼다고 판단

그리고 이러한 일들은 주로 송신측의 윈도우 사이즈가 커 발생하는 일로, 문제상황을 회피하기 위하여 윈도우 크기를 조절하여 혼잡제어를 할 수 있다.

### 혼잡 윈도우

통신을 하기 전, 제일 처음 혼잡 윈도우의 사이즈를 정하기가 까다롭다. 이를 해결하기 위해 MSS(Maximum Segment Size)란 개념이 도입되었으며, 다음과 같은 계산으로 정할 수 있다.

> MSS = Maximum Transmit Unit(시스템에서 제어하는 통신시 최대 단위) - (IP Header + IP Option + TCP Header + TCP Option)

그래서 처음 혼잡제어를 할 때, 송신측은 이러한 MSS를 1로 초기화 한 후 시작하여 윈도우 사이즈를 증가/감소 할 수 있다.

### AIMD

AIMD(Additive Increase, Multiplicative Decrease)는 혼잡상태가 오기 전 MSS를 1씩 증가하다, 혼잡 상태가 오면 현재 윈도우 사이즈의 절반으로 줄이는 방식이다.

간단하게 그래프로 나타내면 다음과 같다

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption><p>Simple AIMD Graph</p></figcaption></figure>

해당 기술은 간단하지만, 생각보다 매울 효율적이다. 만약 새로운 유저가 같은 네트워크 트래픽에 접근했다고 가정을 했을때, 결국 수렴하기 때문이다.

두 유저 기준으로 AIMD의 수렴 상태를 보면 다음과 같다

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption><p>Simple AIMD with two users</p></figcaption></figure>

### Slow Start

하지만 딱봐도 AIMD의 단점이 보인다. 무조건 linear하게 증가하기 때문에 maximum까지 도달하기 까지 꽤나 오랜 시간이 소비된다. 그래서 처음에만 혼잡 윈도우를 2배씩 증가하게 하는 Slow Start라는 기술이 도입되었다.

하지만 Slow Start만으로는 문제가 많은데, 그 이유는 2배씩 증가를 할 때 가끔 라우터의 한계치를 넘는 일이 발생 할 수 있기 때문이다. 그래서 보통 일정 기간까지 slow start로 증가 후 AIMD처럼 linear하게 증가를 하는 방법을 많이 채택을 한다.

여기서 일정 기간이란 `threshold` 라고 하며, 주로 현재의 threshold는 이전에 혼잡 상황이 일어났을 때 윈도우 사이즈를 기준으로 절반으로 지정한다.

이제 그럼 앞서 나온 기술들과 상황을 고려하여 만든 혼잡 제어 알고리즘들을 살펴보자.

### Congestion Control Algorithms

우선 혼잡 제어 알고리즘들에서 혼잡 상황은 3개의 같은 ACK를 받거나, tiemout이 왔을 때로 기준을 정한다. 그리고 다 Slow Start로 시작하여, threshold에 도착하면 MSS를 1씩 증가시키는 linear한 방식을 사용한다.

#### Tahoe

타호같은 경우, ACK를 받아서 혼잡 상태가 된것과 timeout이 된것을 동일하게 본다. 그리고 이러한 혼잡상태가 오면 바로 MSS를 1로 줄여 혼잡 상황을 회피한다.&#x20;

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption><p>simple tahoe graph</p></figcaption></figure>

#### Reno

Reno는 Tahoe와 비슷하지만, 3 ACK Duplicate된 상황과 Timeout된 상황을 구별하는것이 특징이다. 3 ACK가 온 상황에서는, Timeout처럼 큰 문제라고 생각하지 않고 Window Size를 1/2로 줄이게 되고, Timeout이 된 상황에서는 마찬가지로 MSS를 1로 줄이게 된다.

그리고 이렇게 AIMD처럼 window size를 반으로 줄이기 때문에 window 사이즈를 이상적인 크기로 빠르게 복귀할 수 있기에 `Fast Recovery`가 가능하다는 특징이 있다.

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>
