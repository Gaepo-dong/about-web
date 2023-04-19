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

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption><p>Process와 Thread 도식화</p></figcaption></figure>

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

### 병렬 vs 동시

앞서 간략하게 병렬과 동시성 이야기를 했었는데, 조금 깊게 봐야할 필요가 있다.

우선 병렬은 정말 두개가 동시에 일어나는 것이다. 예를들어, 컴퓨터가 4코어 8쓰레드라면, 실제로 최대 4개의 작업은 병렬적으로 진행할 수 있다.&#x20;

하지만 컴퓨터가 동작할때 4개의 작업만 돌아가는것은 아니다. 지금 내가 이 글을 쓰고 있는것만 하여도, OS가 구동되고 있으며, 크롬에서 여러개의 Reference들을 보기위한 탭이 존재하고, VSC에서는 실제로 글을 작성하고, Spotify로 잔잔한 음악도 듣는 등 정말 많은 작업들이 구동되고 있다. 이런것들은 동시(Concurrent)에 돌아가고 있는것이다. 조금 더 컴퓨터 용어에 빗대어 말하자면 큰 작업들이 Context-Switching을 하며 번갈아가며 작업중이다.&#x20;

> 사람이 멀티태스킹의 엄청난 속도로 이루어진다고 보면 된다

그럼 동시성이 왜 필요한지와 Context-Switching이 이루어지는것 까지는 알았다. 그럼 이제 쓰레드의 장점을 알아볼 준비가 되었다..!

### 쓰레드의 장점

#### Process Context-Switching

프로세스의 Context-Switching 과정을 살펴보려면 프로세스를 관리하기 위한 상태정보를 담고있는 자료구조인 PCB(Process Control Block)에 대해서 먼저 살펴봐야한다.

![](<../.gitbook/assets/image (16).png>)

PCB는 대충 이러한 구조를 띄는데 하나씩 분석해보면 다음과 같다.

* Pointer: 프로세스의 현재 위치를 저장하는 포인터 정보
* Process State: 프로세스의 상태 (생성, 준비, 실행, 대기, 종료)
* Process ID: 프로세스에 대한 고유한 ID
* Program Counter  프로세스가 다음에 실행할 명령어의 주소를 저장
* Register: 레지스터 및 범용 레지스터를 포함하는 CPU 레지스터에 있는 정보
* Memory Limits: 운영 체제에서 사용하는 메모리 관리 시스템에 대한 정보
* Open File Lists: 프로세스를 위해 열린 파일 목록

이러한 PCB를 활용하여 Context-Switching을 하면 다음 다이어그램과 같이 실행된다.

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption><p>Simple Process Context-Switching</p></figcaption></figure>

여기서 주요하게 봐야할 점은 PCB1에 내 상태정보를 저장하고, PCB2에서 상태를 가져올 때 overhead가 생기는 것이다. 그리고 이러한 오버헤드는 여기 뿐만이 아닌 다양한 방면에서 일어나는데 주로,&#x20;

* PCB 저장 및 복원
* CPU 캐시 메모리 무효화 (CPU 캐시에 있는 내용을 모두 초기화하고, 새로운 프로세스 정보를 CPU 캐시에 적재해야한다)
* 프로세스 스케줄링

에서 생기게 된다. 그럼 다시 표를 보았을때, context-switching 시 일어나게 되는 overhead 즉 idle한 상태는 다음과 같다.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption><p>idle moment of Process Context Switching</p></figcaption></figure>

#### Thread Context-Switching

먼저 쓰레드가 프로세스안에 어떤식으로 저장되어있는지 보면 다음과 같다.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>Thread in Process</p></figcaption></figure>

그리고 프로세스와 마찬가지로, 쓰레드 또한 Context-Switching을 하기 위해서는 TCB(Thread Control Block)이 존재해야되고, 마찬가지로 Thread ID, 스케줄링 정보 또한 저장된다.&#x20;

> 쓰레드간의 자원공유 및 동기화 또한 TCB에서 관리

여기서 쓰레드 Context-Switching의 장점이 나온다.

1.  TCB가 PCB보다 가벼움

    > TCB에는 stack/register 포인터 등만 저장하기 때문에 PCB보다 가벼워 더 빨리 읽고 쓸수 있다
2.  캐시 메모리를 초기화 하지 않아도 됨

    > 프로세스 컨텍스트 스위칭이 일어날 경우, 다른 프로세스의 실행으로 인해 CPU가 새로운 명령어와 데이터를 로드해야 하기 때문에 CPU 캐시 메모리를 초기화 하여야 하지만, 스레드 컨텍스트 스위칭일 경우, 스택과 레지스터 값 등 일부 컨텍스트 정보만 변경되므로 CPU 캐시 메모리는 초기화되지 않는다

&#x20;

참고

{% embed url="https://www.geeksforgeeks.org/process-table-and-process-control-block-pcb/" %}

{% embed url="https://inpa.tistory.com/entry/%F0%9F%91%A9%E2%80%8D%F0%9F%92%BB-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%E2%9A%94%EF%B8%8F-%EC%93%B0%EB%A0%88%EB%93%9C-%EC%B0%A8%EC%9D%B4" %}
