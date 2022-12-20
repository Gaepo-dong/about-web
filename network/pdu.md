---
description: PDU에 대하여 알아보자
---

# PDU

> Protocol Data Unit

프로토콜 데이터 단위로, 데이터 통신에서 상위 계층이 전달한 데이터에 붙이는 제어 정보

## OSI 모델

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption><p>simple PDU diagram</p></figcaption></figure>

## PDU의 내용

### 전송계층 : Segment

* 발신지 포트
* 목적지 포트
* 순서 번호
* 오류검출코드

### 네트워크 계층 : Packet

* 발신지 컴퓨터 주소
* 목적지 컴퓨터 주소
* 서비스 요청

## SDU

> Service Data Unit

상향/하향 두 통신 계층 간 전달되는 실제 정보

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>simple SDU &#x26; PDU relation</p></figcaption></figure>

{% hint style="info" %}
SDU가 PDU화 하기 전, Fragment / Aggregation이 일어날 수 있음
{% endhint %}



참고

{% embed url="https://en.wikipedia.org/wiki/Protocol_data_unit" %}
