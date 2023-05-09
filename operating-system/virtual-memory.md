---
description: 가상메모리에 대하여 간단하게 알아보자
---

# Virtual Memory

### 가상 메모리

가상 메모리는 왜 나왔을까? 당연히 가상 메모리니까 물리 메모리가 부족해서 나오게 되었다. 만약 가상메모리가 존재하지 않는 상황에서 process의 용량이 RAM의 허용 용량보다 높다면? 당연히 시스템이 터지게 될것이다. 이러한 상황을 가상메모리를 통하여 어떻게 해결할 수 있는지 알아보자

> 실제로 예전에는 프로세스가 RAM의 용량보다 커지기 않게 설정되었다고 한다

가상 메모리의 시스템은, 메모리 주소 0부터 시작하는 개별 프로세스 메모리를 가지고 있다고 생각한다. 그리고 이를 바탕으로, 메모리 관리자가 실제로 사용할 부분만 물리 메모리에 적재하는 방식으로 생각할 수 있다.

<figure><img src="../.gitbook/assets/image (1) (1) (4).png" alt=""><figcaption><p>Virtual Memory vs Main Memory</p></figcaption></figure>

그럼 여기서 Virtual Memory에 어떤 부분을 어떻게 Main Memory에 적재할지 그리고 그에대한 장단점에 대해서 알아보자.

### Frame vs Page

간단하게 요약하자면, Page는 가상 메모리가 가지고 있는 분할 단위이고, Frame은 물리 메모리가 가지고 있는 분할 단위라고 볼 수 있다.

> 서로 같은 크기만큼 분할해야됨에 주의

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>Simple Virtual Memory &#x26; Main Memory</p></figcaption></figure>

이렇게 분할함에 따라, 모든 프로세스에 대한 정보를 메인 메모리에 담지 않아도 되고, 필요한것만 빼면 된다!

여기서 페이징기법의 주소 변환 과정을 보고가면 좋다.

가상주소(페이지)의 주소를 VA \<P, D>라 하고 물리주소(프레임)의 주소를 PA \<F, D>라 하자

> P = Page, F = Frame, D = Distance(얼마나 떨어져있는지)

여기서 가상주소가 2074이고, 페이지의 크기가 1024라 두면

* P = 2074 / 1024 = 2
* D = 2074 % 1024 = 26

그리고 여기서 페이지 테이블을 확인하여 2번 페이지가 5번 프레임에 있다는것을 알면 \<F, D> = <5, 26> 즉 5번 프레임의 26번째로 찾을 수 있다.&#x20;

### TLB

하지만 해당 프레임과 페이지로 찾아갈 때 치명적인 단점이 존재한다. 바로 메인 메모리에 두번 접근해야 된다는것이다.

> 페이지 테이블이 물리 메모리에 존재하기 때문에, 페이지 테이블 접근 후 실제 메모리에 접근해야된다

그래서 결국 명령을 실행할 때 마다, 같은 테이블이지만 메인 메모리에 두번 접근해야되는 오버헤드가 발생하게된다. 그래서 CPU에 TLB (변환색인버퍼)를 두어 페이지 테이블의 캐시 버전을 둬 조금 더 효율적으로 접근할 수 있다.

여기서 매우 유명한 그림을 보면 이해하기가 쉽다

<figure><img src="../.gitbook/assets/image (2) (2).png" alt=""><figcaption><p>Simple TLB</p></figcaption></figure>

이제 이런 구조를 지니고 있을 때 두가지 문제가 생길 수 있다.

1. 단순 TLB miss
2. Page fault

단순 TLB miss는 그렇게 큰 문제는 아니다. 해당 다이어그램을 보면 알 수 있겠지만, main memory에는 올라와있는 상태이므로 page table을 보고 잘 찾아가고, 해당 page table의 내용을 TLB에 기입하면 된다.

하지만 Page fault는 상황이 조금 다르다. 요청한 페이지의 정보가 프레임 즉 메인 메모리에 있지 않은 상태를 나타낸다. Page fault가 났을 때 컴퓨터의 흐름은 다음과 같다.

1. Page fault 발생
2. OS가 page fault trap을 발생
   1. 동작중인 프로세스의 PCB를 메모리에 저장
3. 운영체제가 다른 page table을 확인 (메모리에 없으므로 backing store에서 검색)
4. 빈 프레임을 찾고, page in, page out을 통해 페이지를 교체
   1. 빈 프레임을 찾는동안 CPU는 다른 프로세스를 할당
   2. 찾은 후 사용하던 프로세스의 PCB를 다시 메모리에 저장 후 page fault를 발생시킨 PCB를 복귀
5. page table을 업데이트
6. page fault를 발생시킨 코드를 재실행

여기서 단순 TLB miss인 경우 CPU의 10 cycle정도 걸리지만, page fault는 1,000,000 cycle정보 소비되는 큰 행위임에 주의해야된다



참고&#x20;

{% embed url="https://kjhoon0330.tistory.com/m/entry/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-%EA%B0%80%EC%83%81-%EB%A9%94%EB%AA%A8%EB%A6%AC-1-%EA%B0%80%EC%83%81-%EB%A9%94%EB%AA%A8%EB%A6%AC%EC%99%80-%ED%8E%98%EC%9D%B4%EC%A7%95-%EA%B8%B0%EB%B2%95" %}

{% embed url="https://wpaud16.tistory.com/304" %}
