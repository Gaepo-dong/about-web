---
description: CDN을 알아보자
---

# CDN

> Content Delivery Network

대 콘텐츠 시대에 살고있는 현재 데이터를 지연없이 처리하기 위하여 데이터를 분산하여 전달하는것이 필수가 되었다. 그 기술에 핵심이 되는 CDN에 대하여 알아보자

## CDN 이란

CDN은 지리적 제약 없이 전 세계 사용자들에게 빠르고 안전하게 콘텐츠를 제공하는 기술이라 할 수 있다.

서버와 사용자의 물리적 거리를 줄여 접근 속도를 낮추고, 지역마다 캐시 서버를 배치하여 원본 서버가 아닌 캐시 서버로 요청을보내고 전달받아 트래픽이 몰리는것을 방지할 수 있다. 또한, 한 CDN server에 장애가 난 경우, 다른 CDN으로 전달하여 가용성을 높일 수 있다.

실제 AWS에서 제공하는 Cloud Front라는 CDN 솔루션을 이용하면, 다음과 같이 수많은 캐시 서버를 이용할 수 있다.



<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>Acutal CloudFront CDN edge locations</p></figcaption></figure>

## 동작 원리

1. 최초 요청은 서버로 콘텐츠를 가져와 CDN 캐시 장비에 저장
2. 두번째 이후 요청은 CDN 솔루션에서 지정하는 콘텐츠 만료 시점까지 CDN 캐싱 장비에 저장된 콘텐츠를 제공
3. 자주사용하는 페이지에 CDN 캐싱이 되며, 콘텐츠 호출이 없다면 주기적으로 삭제하여 관리
4. 서버가 파일을 불러오지 못하는 경우, 다른 CDN 서버에서 찾아 Client에게 응답을 전송
5. 콘텐츠를 사용할 수 없거나, 오래된 경우, Proxy를 사용하여 다음 요청에 응답할 수 있는 새로운 콘텐츠를 저장

## 캐싱 방식

### Static Caching

* Origin Server에 있는 콘텐츠를 운영자가 미리 cache server에 복사해두어 사용

### Dynamic Caching

* 사용자가 콘텐츠 요청 시, 콘텐츠가 없는 경우 Origin Server로부터 다운받아 전달
  * 콘텐츠가 있는 경우 캐싱된 콘텐츠 전달
* 각각의 콘텐츠는 일정 시간 이후 캐시 서버에서 삭제 될 수 있음

> AWS Cloud Front는 static caching, dynamic caching 모두 지원한다

