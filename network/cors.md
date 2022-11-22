---
description: CORS 박살내기
---

# CORS

## Origin

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p>URL구성요소</p></figcaption></figure>

URI에서 Protocol, Host, Port부분을 origin이라 부른다.

{% content-ref url="url-vs-uri-vs-urn.md" %}
[url-vs-uri-vs-urn.md](url-vs-uri-vs-urn.md)
{% endcontent-ref %}

그리고 해당 origin이 같은 경우 SOP에 부합하기에 CORS가 허용된다.

## SOP & CORS

> SOP(Same Origin Policy), CORS(Cross Origin Resource Sharing)

앞서 말했듯이 SOP는 다른 Origin으로 요청을 보낼 수 없도록 금지하는 브라우저의 기본 보안 정책이다. 예전에는, 다른 Origin으로 보내는 것이 엄격하게 금지되었지만, 몇가지 예외 사항이 있다.

1. `<script>` 태그를 활용하는 경우
2. 이미지 렌더링을 하는 경우
3. `<link>` 태그를 사용하는 경우
4. HTML 문서를 화면에 띄우는 경우
5. CORS 정책을 지키는 요청을 하는 경우

즉 CORS는 다른 Origin으로 요청을 보내기 위한 정책 + SOP를 풀어주는 정책 이라 할 수 있다.

{% hint style="info" %}
CORS 는 브라우저 정책임에 주의
{% endhint %}

> 기본적인 CSRF 공격을 방어해준다

## CORS flow

> 다른 Origin으로 요청 보낼 시 Origin 헤더에 자신의 Origin을 설정하여, 서버로부터 응답을 할 때 `Access-Control-Allow-Origin`헤더의 목록에 포함되어 있는지 검사

### Simple Request

> 안전한 요청으로 취급이 되며, Preflight 없는 한 번의 요청으로 전송이 되는 경우

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption><p>Simple Request</p></figcaption></figure>

하지만 Simple Request를 만족시키는 조건이 꽤나 까다롭다

* Method가 GET, HEAD, POST 중 하나
* 아래와 같은 헤더만 사용
  * Accept
  * Accept-Language
  * Content-Language
  * Content-Type
    * application/x-www-form-urlencode
    * multipart/form-data
    * text/plain
  * DPR
  * Downlink
  * Save-Data
  * Viewport-Width
  * Width\


### Preflight Request

앞에 언급한 Simple Request가 아닌 경우, 안전한 요청인지 확인하는 Preflight 과정이 필요하게 된다.

<figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption><p>Preflight Request Flow</p></figcaption></figure>

Preflight Request의 특징은 다음과 같다

* Method로 OPTIONS 사용
* Origin헤더에 자신의 Origin 설정
* Access-Control-Request-Method 헤더에 실제 사용할 메소드 설정
* Access-Control-Request-Headers에 실제 사용할 헤더 설정

Preflight Respond는 다음과 같다

* Access-Control-Allow-Origin 헤더에 허용되는 Origin의 목록 및 `*`을 설정
* Access-Control-Allow-Methods 헤더에 허용되는 메소드 혹은 `*` 을 설정
* Access-Control-Allow-Headers 헤더에 허용되는 목록 혹은 `*` 을 설정
* Access-Control-Max-Age 헤더에 해당 Preflight요청이 캐시될 수 있는 최대 시간을 설정

해당 응답을 받으면 브라우저는 자신이 전송한 요청과 비교하여 안전성을 검사할 수 있다.

### Credential Request

일반적으로 Cookie 혹은 Authorization 헤더에 존재하는 토큰은 별도로 지정해주지 않는한 전송하지 않는다. 그래서 별도로 CORS 정책을 설정해주어야 된다.

XMLHttpRequest, ajax, axios 등은 `withCredentials` 옵션을 `true` 로 설정해주면 되고, `fetch` 인 경우 `credentials` 옵션을 `include` 로 설정해주어야 된다.

{% hint style="info" %}
Access-Control-Allow-Origin 헤더가 `*` 가 아닌, 분명한 Origin으로 설정되고, 서버의 Access-Control-Allow-Credentials 헤더가 `true` 로 설정되어야 함에 주의
{% endhint %}

<figure><img src="../.gitbook/assets/image (1) (2).png" alt=""><figcaption><p>Credential Request flow</p></figcaption></figure>





참고

{% embed url="https://it-eldorado.tistory.com/163" %}
