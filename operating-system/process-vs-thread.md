---
description: OS의 기본
---

# Process vs Thread

> Process: OS로부터 할당받은 작업의 단위
>
> Thread: 프로세스가 할당받은 자원을 이용하는 실행흐름의 단위

프로그램을 실행시키면 프로세스가 실행되는것은 간단 명료한 사실이다. 하지만, 왜 굳이 프로세스를 쪼개어 쓰레드로 사용할까?

### 쓰레드의 탄생배경

컴퓨터가 처음 탄생했을 때는, 프로그램당 하나의 프로세스만 동작하였다. 정말 컴퓨터에서 한 동작만 한다면 문제가 되지 않지만, 실제로는 그렇지 않다. 만약 브라우저에서 파일을 다운로드 받고 있을 때 브라우저가 `지금 이거 다운로드 받고 있으니 다른거 하지말고 기다려!` 라고 한다면 누가 가만히 기다릴 수 있을까? 과연 유튜브나 인터넷 서핑없이 다운로드를 기다릴 수 있을까..? 이걸 해결해주는것이 쓰레드, 프로세스 그리고 더 나아가 병렬과 동시성이다.

### 쓰레드 훑어보기

쓰레드는 영어로 Thread 라, 단순히 `실` 이라고 번역하는 사람들이 많다. 하지만 Thread는 실타래 말고 `a line of reasoning or train of thought that connects the parts in a sequence` 이라는 뜻을 지니고 있고, 여러 연속된 것들을 묶어주는 의미를 지니고 있다. 즉 프로세스 내에서 동시에 진행되는 작업 갈래, 흐름의 단위라고 볼 수 있다.&#x20;

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>Process와 Thread 도식화</p></figcaption></figure>

사진을 보면 Process에 main thread 를 작성해두었는데, 한 프로세스에는 적어도 하나의 main thread가 존재해야되고, 다른 thread들을 관리하는 control tower라 볼 수 있다.



## Process vs Thread

### Process의 구조

Process와 Thread의 비교를 하기 전, 우선 Process의 구조에 대한 이해를 먼저 해야된다.

![](<../.gitbook/assets/image (2).png>)

많이들 봤을듯한 다이어그램으로 시작하면, 크게 4가지 영역이 보인다.

1. Code / Text: 프로그램 함수들의 코드가 기계어로 저장된 공간
2. Data: 코드가 실행되며 사용되는 전역변수 및 데이터들이 저장된 공간
3. Heap: 생성자 및 인스턴스같이 런타임에 동적으로 할당되는 데이터들을 저장하는 공간
4. Stack: 지역변수와 같이 함수가 종료되면 돌아올 자료들을 저장하는 독립적인 공간 (함수호출과 동시에 할당되어, 리턴시에 반납)

이러한 영역들이 매 프로세스가 생성될 때 마다 생성되는 블럭이라 볼 수 있다.

### Thread의 자원 공유

위에서 Stack 공간을 설명할 때, `독립적인 공간` 이라 설명한 바가 있다. 그 이유가 여기서 나오는데, 쓰레드끼리는 서로 자원을 공유하지만, 유일하게 Stack 공간만 독립적으로 가진다. 그 이유를 보기위해서는 stack공간을 조금더 유심히 봐야된다.

> stack은 함수 호출 시 전달되는 인자, 되돌아갈 주소값, 함수 내에서 선언하는 변수 등을 저장하는 메모리 공간이기 때문에, 독립적인 스택을 가졌다는 것은 독립적인 함수 호출이 가능하다

즉 Stack만 존재하여도, 독립적인 함수 호출이 가능하며, 독립적인 실행 흐름이 추가될 수 있다는 뜻이다.&#x20;

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>Process와 Thread 도식화</p></figcaption></figure>

> 프로세스의 자원공유는 어떻게 할 수 있을까?
>
> 1. IPC (Inter Process Communication) : Message Passing으로 전달
> 2. LPL (Local inter-Process Communication) : shared memory 공간을 활용하여 데이터 전달
