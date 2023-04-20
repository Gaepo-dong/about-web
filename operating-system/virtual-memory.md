---
description: 가상메모리에 대하여 간단하게 알아보자
---

# Virtual Memory

### 가상 메모리

가상 메모리는 왜 나왔을까? 당연히 가상 메모리니까 물리 메모리가 부족해서 나오게 되었다. 만약 가상메모리가 존재하지 않는 상황에서 process의 용량이 RAM의 허용 용량보다 높다면? 당연히 시스템이 터지게 될것이다. 이러한 상황을 가상메모리를 통하여 어떻게 해결할 수 있는지 알아보자

> 실제로 예전에는 프로세스가 RAM의 용량보다 커지기 않게 설정되었다고 한다

가상 메모리의 시스템은, 메모리 주소 0부터 시작하는 개별 프로세스 메모리를 가지고 있다고 생각한다. 그리고 이를 바탕으로, 메모리 관리자가 실제로 사용할 부분만 물리 메모리에 적재하는 방식으로 생각할 수 있다.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>Virtual Memory vs Main Memory</p></figcaption></figure>

그럼 여기서 Virtual Memory에 어떤 부분을 어떻게 Main Memory에 적재할지 그리고 그에대한 장단점에 대해서 알아보자.

### Frame vs Page

간단하게 요약하자면, Page는 가상 메모리가 가지고 있는 분할 단위이고, Frame은 물리 메모리가 가지고 있는 분할 단위라고 볼 수 있다.

> 서로 같은 크기만큼 분할해야됨에 주의

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>Simple Virtual Memory &#x26; Main Memory</p></figcaption></figure>







참고&#x20;

{% embed url="https://kjhoon0330.tistory.com/m/entry/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-%EA%B0%80%EC%83%81-%EB%A9%94%EB%AA%A8%EB%A6%AC-1-%EA%B0%80%EC%83%81-%EB%A9%94%EB%AA%A8%EB%A6%AC%EC%99%80-%ED%8E%98%EC%9D%B4%EC%A7%95-%EA%B8%B0%EB%B2%95" %}

{% embed url="https://wpaud16.tistory.com/304" %}
