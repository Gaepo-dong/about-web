---
description: Page Fault가 쏘아올린 작은 공
---

# Thrashing

앞서 Virtual Memory에서 Page Fault가 cycle을 되게 많이 잡아먹어 CPU 이용률에 좋지 않다는것을 알았다.

> \>> [Virtual Memory 복습하기](https://gaepodong.gitbook.io/about-web/operating-system/virtual-memory)

이러한 Page Fault가 연속적으로 일어났을 때 Thrashing이라는 나쁜 결과가 나올 수 있는데, 한번 알아보자

### Thrashing의 원인

CPU의 이용률이 떨어지면, 컴퓨터는 자동으로 멀티 프로그래밍의 정도를 높이게 된다. 하지만, 멀티 프로그래밍의 정도가 높아진다는 뜻은, 그만큼 한 메인 메모리에 여러개의 프로세스들이 존재하기 때문에 자연스럽게 Page Fault의 빈도가 높아질 수 밖에 없다. 그리고 Page Fault가 빈번하게 일어남에 따라 Swap 영역에서 해당 페이지를 메모리로 가져오기 위해 디스크 I/O 작업이 발생하고 CPU는 다른 프로세스에게 할당된다.

결국 Ready Queue에 있는 모든 프로세스에게 CPU가 한 차례씩 할당되었는데도 자신의 차례가 오는 순간, 각 프로세스들이 페이지 부재를 발생시켜 시스템은 페이지 부재를 처리하느라 매우 분주해지고 CPU 이용률은 오히려 떨어지게 되는 현상이다.

> 국룰 사진으로 보면 이해가 더 쉽다

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Simple Thrashing</p></figcaption></figure>

### 극복 방안

#### Working Set Algorithm / Locality

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption><p>Simple Working Set Diagram</p></figcaption></figure>

> 프로세스가 많이 참조하는 페이지 집합을 메모리 공간에 계속 상주시켜 쓰레싱을 줄이는 방법

즉 자주 사용되는 페이지들을 워킹셋으로 묶어 지역성을 활용하여 메모리에 동시에 올리는 작업이다. 해당 워킹셋의 합인 페이지가 할당된 프레임보다 커지게 되면 쓰레싱이 발생하게 된다.

#### 페이지 부재 빈도 Algorithm

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>Simple Page Fault Frequency Alogirhtm</p></figcaption></figure>

프로세스의 Page Fault를 주기적으로 조사하며 각 프로세스에 할당할 메모리 양을 동적으로 예측하고 조절하는 알고리즘이다.&#x20;

해당 다이어그램에서 볼 수 있듯, Page Fault가 높다면(Upper Bound기준) Frame Count를 늘려 Page Fault를 낮추고, 반대로 너무 낮아도, Frame Count를 줄이며(잘 사용하지 않는 프레임 회수) 조절한다.\


참고

{% embed url="https://zangzangs.tistory.com/144" %}
