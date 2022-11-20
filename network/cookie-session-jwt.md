---
description: Cookie, Session 더 나아가 JWT까지
---

# Cookie, Session, JWT

해당 개념을 보기전 HTTP의 특징을 복기해보자

## HTTP의 특징

### Connectionless

> HTTP의 특징으로, 클라이언트와 서버가 요청과 응답을 각 주고 받은 후 연결을 끊어버리는 특징

### Stateless

> 요청과 응답을 통하여 통신이 끝난 후 상태 정보를 유지하지 않는 특징

이러한 특징의 장단점은 다음과 같다

### 장점

* 서버가 다수의 클라이언트와 연결을 유지할수록 자원 낭비가 심해진다
* 비연결성과 무상태성을 이용하여 불필요한 자원의 낭비를 줄일 수 있다

### 단점

* 서버는 클라이언트를 식별 할 수 없다
* 매번 재 로그인을 해야된다

그리고 이러한 단점을 보완할 수 있는것이 Cookie, Session, JWT이다.

## Cookie

> 웹사이트를 방문할 경우, 사이트가 사용하고있는 서버를 통해 클라이언트의 브라우저에 설치되는 정보파일

{% hint style="info" %}
localStorage와 헷갈리지 않게 주의!

localStorage : 영구적으로 저장되며, HTTP요청에 매번 전송하지 않아도 되는 5MB의 브라우저 저장공간

Cookies : persistent cookie와 session cookie로 나뉘며, 4KB의 저장공간을 지님

* persistent cookie : 만료일을 가짐
* session cookie : 브라우저 닫으면 삭제
{% endhint %}

### Flow

1. 서버가 클라이언트에게 응답을 작성하라고 set-cookie요청을 보낸다 `Headers = [Set-Cookie: "user=yeonggi", "password=pw"]`
2. 클라이언트는 요청을 보낼 때 마다, 저장된 쿠리를 헤더에 담아 전송 `Headers = [Cookie:"user=yeonggi", "password=pw"]`
3. 서버가 쿠키에 담긴 정보를 바탕으로 클라이언트를 식별

### 단점

* 보안에 취약하다
  * 요청 시 쿠키의 값을 그대로 보냄
  * 유출 및 조작당할 수 있음
* 용량이 제한적이라 많은 정보를 담을 수 없음
* 브라우저마다 쿠키에대한 지원이 달라, 브라우저간 공유가 불가능
* 쿠키의 사이즈가 커질수록 네트워크의 부하가 심함

## Session

> 비밀번호 등 클라이언트의 인증 정보를 쿠키가 아닌 서버측에 저장 및 관리하여 쿠키가 유출 및 조작 당할 위험을 줄임

### Flow

1. 서버가 클라이언트의 로그인 요청에 응답을 작성할 때, 인증정보를 서버에 저장 후 클라이언트 식별자인 JSSESSIONID를 쿠키에 저장 `Set-Cookie: JSESSIONID=LSKDJFLKSDJFLKSDJF87293SDKJF`
2. 클라이언트가 요청을 보낼 때 마다 JSESSIONID 쿠키를 전송
3. 서버가 JSESSIONID의 유효성을 판별 및 클라이언트 식별

### 장점

* 쿠키를 포함한 요청이 외부에 유출되어도 세션ID 자체는 유의미한 정보가 아님
* 각 사용자가 고유한 세션ID를 가지므로, 요청이 들어올 때 마다 회원정보를 확인할 필요가 없음

### 단점

* 쿠키를 탈취 후 클라이언트인척 위장 할 수 있음
* 서버에서 세션 저장소가 있으므로, 요청이 많을수록 서버에 부하가 심함

## JWT (JASON Web Token)

> 인증에 필요한 정보들을 암호화한 토큰으로, HTTP 헤더에 실어 서버가 클라이언트를 식별

### 구조

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>jwt.io를 활용하여 만든 임시 JWT</p></figcaption></figure>

해당 JWT를 parsing 하면 다음과 같은 구조를 가지고 있다.

#### Header

```
{
  "alg": "HS256",
  "typ": "JWT"
}
```

암호화할 해싱 알고리즘 및 토큰의 타입을 지정

#### Payload

```
{
  "name": "yeonggi",
  "iat": 19980909
}
```

토큰에 담을 정보로, 클라이언트의 고유ID와 유효기간등이 포함되어, key-value로 이루어짐

#### Signature

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  
your-256-bit-secret

) secret base64 encoded
```

인코딩된 Header와 Payload를 더한 뒤 비밀키로 해싱한 구조로, 서버측에서 관리하는 비밀키가 유출되지 않는 한 복호화 할 수 없고, 토큰의 위변주 여부를 확인할 수 있음

### Flow

1. 클라이언트의 요청이 들어오면, 서버가 검증 후 클라이언트 고유ID를 포함한 정보를 payload에 저장
2. 서버의 비밀키를 활용하여 Access Token(JWT)를 발급
3. 클라이언트가 토큰을 저장해두고, 서버에 요청할 때 마다 요청헤더 `Authorization` 에 포함하여 전달
4. 토큰의 Signature를 비밀키로 복호화하여, 위변조 및 유효기간을 확인
5. 유효한 토큰인 경우 요청에 응답

### 장점

* Header와 Payload를 가지고 Signature를 생성하여 데이터의 위변조를 막을 수 있음
* 인증정보에 대한 저장소가 필요 없음
* 검증됐음을 증명하는 서명 및 필요한 정보를 모두 지님
* 서버가 무상태로 유지할 수 있음
* 확장성이 우수함
* 토큰 기반의 다른 로그인 시스템에 접근 및 권한 공유가 쉬움
* 모바일 어플리케이션에서도 잘 작동함

### 단점

* JWT 토큰의 길이가 길어, 네트워크의 부하가 심해짐
* Payload가 암호화 되지는 않아, 유저의 중요한 정보를 담을 수 없음
* 토큰을 탈취당하면 대처하기 어려움
* 한번 발급되면, 유효기간이 만료될 때 까지 사용이 가능함
* 특정 사용자의 접속을 만료할 수 없음

### 보안전략

#### Access Token

사용자가 로그인 할 때, 지정된 기간동안 사용할 수 있는 Token을 발급할 수 있다.&#x20;

만료 시간을 짧게 할수록 보안이 강화되지만, 자주 로그인을 해야된다.

#### Sliding Sessions

유효한 AccessToken을 가진 클라이언트의 요청에 새로운 AccessToken을 발급하는 방법이다.&#x20;

지속적으로 이용하는 클라이언트에게 자동으로 토큰 만료 기한을 늘려주는 방법으로, Access Token에서 자주 로그인을 해야되는 단점을 보완한다.

#### Refresh Token

Access Token과 Refresh Token을 클라이언트에게 함께 발급해주는 방법으로, Access Token은 만료 기간을 짧게, Refresh Token은 길게 설정한다.

클라이언트가 Access Token이 만료되면, Refresh Token을 활용하여 자동으로 Access Token을 재발급하는 방식이다.

주로 Refresh Token은 서버의 Storage에 저장하여 검증에 활용하므로, 강제로 토큰을 만료도 가능하다. 하지만 Refresh Token을 활용할수록 I/O 작업이 필수적이다.



참고

{% embed url="https://tecoble.techcourse.co.kr/post/2021-05-22-cookie-session-jwt/" %}

{% embed url="https://erwinousy.medium.com/%EC%BF%A0%ED%82%A4-vs-%EB%A1%9C%EC%BB%AC%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C-28b8db2ca7b2" %}
