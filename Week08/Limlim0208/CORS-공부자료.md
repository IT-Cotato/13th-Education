# 공부 자료

<aside>

</aside>

---

[교육 목표]

1. 웹의 Origin Policy
2. CORS의 개념
3. CORS 시나리오
4. CORS 에러 대응법

---

# **웹의 Origin(출처) Policy**

![URL의 구성요소, [https://velog.io/@rokwon_k/Web-CORS](https://velog.io/@rokwon_k/Web-CORS)](image.png)

URL의 구성요소, [https://velog.io/@rokwon_k/Web-CORS](https://velog.io/@rokwon_k/Web-CORS)

출처를 구성하는 세 요소: 프로토콜·도메인(호스트 이름)·포트

> `Protocol + Host + Path`= `Origin`
> 

이 중 하나라도 다르면 다른 출처

ex)  [https://naver.com](https://naver.com/)과 [http://naver.com](http://naver.com/) 은 프로토콜이 다르므로 다른 출처

웹은 크게 두가지의 Origin 정책을 가지고 있음

1. SOP
2. CORS

---

# SOP(Same-Origin Policy, 동일 출처 정책)

> SOP(Same-Origin Policy)
: 서로 다른 출처일 때 리소스 요청과 응답을 차단하는 정책
> 
- 브라우저는 기본적으로 다른 서버를 신뢰하지 않음
- 다른 서버에 요청을 보내거나 응답을 받는 걸 차단
- 토큰, 쿠키와 같이 민감한 사용자 정보가 브라우저에 저장되는데, 이 정보를 탈취하면 심각한 보안 이슈가 생기기 때문
- 서로 다른 서버 리소스는 공유하지 않는 브라우저 정책을 SOP(Same-Origin Policy, 동일 출처 정책)라고 함

**에러 발생 예시**

![[https://docs.tosspayments.com/blog/payment-window-cors-error](https://docs.tosspayments.com/blog/payment-window-cors-error)](image%201.png)

[https://docs.tosspayments.com/blog/payment-window-cors-error](https://docs.tosspayments.com/blog/payment-window-cors-error)

<aside>

1. 브라우저에서 `https://myshop.com`의 출처를 가진 웹 애플리케이션에서 `https://othershop.com` 출처의 리소스를 요청함
2. `https://othershop.com` 출처에서는 `https://myshop.com`출처의 리소스 요청을 거부함
3. CORS 에러 발생!

![image.png](image%202.png)

</aside>

**SOP에 부합하는 URL 예시**

<aside>

도메인(Hostname): [cotato.kr](http://cotato.kr/)
출처(Origin): [https://www.cotato.kr/](https://www.cotato.kr/)

</aside>

![[https://docs.tosspayments.com/resources/glossary/cors](https://docs.tosspayments.com/resources/glossary/cors)](image%203.png)

[https://docs.tosspayments.com/resources/glossary/cors](https://docs.tosspayments.com/resources/glossary/cors)

| **URL** | 동일한 출처인가? |
| --- | --- |
| **https://www.cotato.kr:443/** | ✅ 프로토콜, 도메인, 포트가 같음
HTTPS의 기본 포트인 443은 생략 가능 |
| **https://www.cotato.kr/example2/** | ✅ 경로는 오리진에 포함되지 않아 같음 |
| **http://www.cotato.kr/example/** | ❌ 프로토콜이 다름 |
| **https://en.cotato.kr/example/** | ❌ 도메인이 다름 |
| **https://www.cotato.kr:8080/example/** | ❌ 포트가 다름 |
| **http://en.cotato.kr:8080/example/** | ❌ 프로토콜, 도메인, 포트가 다름 |

---

# 웹의 발달과 CORS

**과거**

- 프론트엔드와 백엔드를 따로 구성하지 않고 한 번에 구성해서 모든 처리가 같은 도메인 안에서 가능했음
- 다른 출처로 요청을 보내는 게 의심스러운 행위로 보일 수 밖에 없었음

**현재**

- 시간이 지나 클라이언트에서 API를 직접 호출하는 방식이 당연해짐
- 보통 클라이언트와 API는 다른 도메인에 있는 경우가 다수

**→ CORS 정책이 생김**

출처가 다르더라도 요청과 응답을 주고받을 수 있도록, 서버에 리소스 호출이 허용된 출처(Origin)를 명시해 주는 방식

---

# **CORS**(Cross-Origin Resource Sharing, **교차 출처 리소스 공유 정책)**

> CORS(Cross-Origin Resource Sharing) **교차 출처 리소스 공유**
: 서로 다른 출처라도 리소스 요청, 응답을 허용할 수 있도록 하는 정책
> 
- CORS 한마디 정리: 서로 다른 서버끼리 리소스를 공유하기 위한 정책

리소스 요청, 응답을 허용할지 결정하는 브라우저의 검증과 허락, 그를 위한 HTTP 헤더 사용 등을 포함

- 즉, ‘출처가 교차한다’는 건 리소스를 주고받으려는 ‘두 출처가 서로 다르다’는 뜻
- CORS를 설정 → ‘출처가 다른 서버 간의 리소스 공유’를 허용한다는 것
- 서버측에서 헤더를 통해서 다른 출처(웹)에서 자원에 접근할 수 있는 권한을 부여하도록 브라우저에게 알려주는 정책

**브라우저가 CORS를 확인하는 방법**

<aside>

1. 웹에서 서버로 요청
2. 브라우저에서 요청 헤더에 `Origin`을 추가로 담아서 보낸다.
3. 서버에서는 보내는 응답 헤더 안에 `Access-Control-Allow-Origin` 넣어서 보낸다.
4. 브라우저는 요청시 보낸 `Origin`과 응답 헤더 안에 담긴 `Access-Control-Allow-Origin`의 값을 비교한다
5. 만약 `Access-Control-Allow-Origin`안에 `Origin`이 포함되지 않는다면 브라우저가 해당 응답을 버리고 **CORS Policy를 위반**했다는 에러를 console에 뿌려준다.
</aside>

- 클라이언트-서버의 소통은 정상적으로 성공하였으므로 서버의 로그에도 정상적으로 응답했다고 찍힌다.

---

# CORS 시나리오

CORS는 상황에 따라서 다음 3가지 요청방식을 사용함

1. 사전 요청(Preflighted requests)
2. 단순 요청(Simple requests)
3. 자격 증명 요청 (Credentialed Request)

## **단순 요청(Simple requests)**

사전요청(Prefilght)을 생략하고 바로 서버에 직행으로 본 요청을 보낸 후, 서버가 이에 대한 응답의 헤더에 Access-Control-Allow-Origin 헤더를 보내주면 브라우저가 CORS정책 위반 여부를 검사하는 방식

![[https://evan-moon.github.io/2020/05/21/about-cors/](https://evan-moon.github.io/2020/05/21/about-cors/)](image%204.png)

[https://evan-moon.github.io/2020/05/21/about-cors/](https://evan-moon.github.io/2020/05/21/about-cors/)

![image.png](image%205.png)

대표적으로 아래 **3가지 경우를 만족** 할때만 가능

1. 요청의 메소드는 GET, HEAD, POST 중 하나여야 한다.
2. Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width, Width 헤더일 경우 에만 적용된다.
3. Content-Type 헤더가 application/x-www-form-urlencoded, multipart/form-data, text/plain중 하나여야한다. 아닐 경우 예비 요청으로 동작된다.

위 조건을 모두 만족되어 단순 요청이 일어나는 상황은 드물다.

왜냐하면 대부분 HTTP API 요청은 text/xml 이나 application/json 으로 통신하기 때문에 3번째 Content-Type이 위반되기 때문이다.

따라서 대부분의 API 요청은 그냥 예비 요청(preflight)으로 이루어진다.

## 사전 요청(Preflighted requests)

브라우저는 본 요청을 하기 전에 Preflight Request(예비 요청)를 보내 서버와 잘 통신할 수 있는지 확인하는 요청을 보냅니다.

예비 요청의 역할: 본 요청을 보내기 전에 브라우저 스스로 안전한 요청인지 미리 확인하는 것

**특징**

- 예비요청의 HTTP 메소드를 GET이나 POST가 아닌 OPTIONS라는 요청이 사용됨
- 우리가 자바스크립트의 fetch API를 사용하여 브라우저에게 리소스를 받아오라는 명령을 내리면 브라우저는 서버에게 예비 요청을 먼저 보내고, 서버는 이 예비 요청에 대한 응답으로 현재 자신이 어떤 것들을 허용하고, 어떤 것들을 금지하고 있는지에 대한 정보를 응답 헤더에 담아서 브라우저에게 다시 보내주게 된다.
- 이후 브라우저는 자신이 보낸 예비 요청과 서버가 응답에 담아준 허용 정책을 비교한 후, 이 요청을 보내는 것이 안전하다고 판단되면 같은 엔드포인트로 다시 본 요청을 보내게 된다. 이후 서버가 이 본 요청에 대한 응답을 하면 브라우저는 최종적으로 이 응답 데이터를 자바스크립트에게 넘겨준다.

![[https://evan-moon.github.io/2020/05/21/about-cors/](https://evan-moon.github.io/2020/05/21/about-cors/)](image%206.png)

[https://evan-moon.github.io/2020/05/21/about-cors/](https://evan-moon.github.io/2020/05/21/about-cors/)

**예시**

![[https://inpa.tistory.com/entry/WEB-📚-CORS-💯-정리-해결-방법-👏#cors_작동_방식_3가지_시나리오](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-CORS-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%F0%9F%91%8F#cors_%EC%9E%91%EB%8F%99_%EB%B0%A9%EC%8B%9D_3%EA%B0%80%EC%A7%80_%EC%8B%9C%EB%82%98%EB%A6%AC%EC%98%A4)](image%207.png)

[https://inpa.tistory.com/entry/WEB-📚-CORS-💯-정리-해결-방법-👏#cors_작동_방식_3가지_시나리오](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-CORS-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%F0%9F%91%8F#cors_%EC%9E%91%EB%8F%99_%EB%B0%A9%EC%8B%9D_3%EA%B0%80%EC%A7%80_%EC%8B%9C%EB%82%98%EB%A6%AC%EC%98%A4)

<aside>

1. 자바스크립트의 fetch() 메서드를 통해 리소스를 받아오려고 한다.
2. 브라우저는 서버로 HTTP OPTIONS 메소드로 예비 요청(Preflight)을 먼저 보낸다.
    1. Origin 헤더에 자신의 출처를 넣는다.
    2. Access-Control-Request-Method 헤더에 실제 요청에 사용할 메소드를 설정한다.
    3. Access-Control-Request-Headers 헤더에 실제 요청에 사용할 헤더들을 설정한다.
3. 서버는 이 예비 요청에 대한 응답으로 어떤 것을 허용하고 어떤것을 금지하고 있는지에 대한 헤더 정보를 담아서 브라우저로 보내준다.
    1. Access-Control-Allow-Origin 헤더에 허용되는 Origin들의 목록을 설정한다.
    2. Access-Control-Allow-Methods 헤더에 허용되는 메소드들의 목록을 설정한다.
    3. Access-Control-Allow-Headers 헤더에 허용되는 헤더들의 목록을 설정한다.
    4. Access-Control-Max-Age 헤더에 해당 예비 요청이 브라우저에 캐시 될 수 있는 시간을 초 단위로 설정한다.
4. 이후 브라우저는 보낸 요청과 서버가 응답해준 정책을 비교하여, 해당 요청이 안전한지 확인하고 본 요청을 보내게 된다.
5. 서버가 본 요청에 대한 응답을 하면 최종적으로 이 응답 데이터를 자바스립트로 넘겨준다.
</aside>

단점

- 요청을 보내기 전에 OPTIONS 메서드로 예비 요청을 보내 보안을 강화
- 실제 요청에 걸리는 시간이 늘어나게 되어 어플리케이션 성능에 영향을 미침
    - 수행하는 API 호출 수가 많으면 많을 수록 예비 요청으로 인해 서버 요청을 배로 보내게 되니 비용 상승
    - 브라우저 캐시를 이용해 Access-Control-Max-Age 헤더에 캐시될 시간을 명시해 주면, 이 Preflight 요청을 캐싱 시켜 최적화 가능

## **자격 증명 요청 (Credentialed Request)**

클라이언트에서 서버에게 자격 인증 정보(Credential)를 실어 요청할때 사용되는 요청

자격 인증 정보: 세션 ID가 저장되어있는 쿠키(Cookie) 혹은 Authorization 헤더에 설정하는 토큰 값 등

즉, 클라이언트에서 일반적인 JSON 데이터 외에도 쿠키 같은 인증 정보를 포함해서 다른 출처의 서버로 전달할때 자격 증명 요청으로 동작한다.

이는 기존의 단순 요청이나 예비 요청과는 살짝 다른 인증 형태로 통신하게 된다.

#### **1. 클라이언트에서 인증 정보를 보내도록 설정하기**

기본적으로 브라우저가 제공하는 요청 API 들은 별도의 옵션 없이 브라우저의 쿠키와 같은 인증과 관련된 데이터를 함부로 요청 데이터에 담지 않도록 되어있다.
이때 요청에 인증과 관련된 정보를 담을 수 있게 해주는 옵션이 바로 credentials 옵션이다. 이 옵션에는 3가지의 값을 사용할 수 있으며, 각 값들이 가지는 의미는 아래와 같다.

| 옵션 값 | 설명 |
| --- | --- |
| same-origin(기본값) | 같은 출처 요청에만 인증 정보 포함 |
| include | 모든 요청에 인증 정보 포함 |
| omit | 모든 요청에 인증 정보를 포함하지 않음 |

만일 이러한 별도의 설정을 해주지 않으면 쿠키 등의 인증 정보는 절대로 자동으로 서버에게 전송되지 않는다.

서버에 인증된 요청을 보내는 방법으로는 fetch 메서드를 사용하거나 axios, jQuery 라이브리리 등 다양하다.

**fetch 메서드 예시**

```jsx
// fetch 메서드
fetch("https://example.com:1234/users/login", {
	method: "POST",
	credentials: "include",
	// 클라이언트와 서버가 통신할때
	// 쿠키와 같은 인증 정보 값을 공유하겠다는 설정
    body: JSON.stringify({
        userId: 1,
    }),
})
```

#### 2. 서버에서 인증된 요청에 대한 헤더 설정하기

서버도 마찬가지로 이러한 인증된 요청에 대해 일반적인 CORS 요청과는 다르게 대응해줘야 한다.

1. *`Access-Control-Allow-Origin`*에는 를 사용할 수 없으며, 명시적인 URL이어야한다.
2. 응답 헤더에는 반드시 **`Access-Control-Allow-Credentials: true`*가 존재해야한다.

<aside>

1. 응답 헤더의 Access-Control-Allow-Credentials 항목을 true로 설정해야 한다.
2. 응답 헤더의 Access-Control-Allow-Origin 의 값에 와일드카드 문자("*")는 사용할 수 없다.*
3. *응답 헤더의 Access-Control-Allow-Methods 의 값에 와일드카드 문자("*")는 사용할 수 없다.
4. 응답 헤더의 Access-Control-Allow-Headers 의 값에 와일드카드 문자("*")는 사용할 수 없다.
</aside>

즉, 응답의 Access-Control-Allow-Origin 헤더가 와일드카드(*)가 아닌 분명한 Origin으로 설정되어야 하고, Access-Control-Allow-Credentials 헤더는 true로 설정되어야 한다는 뜻이다. 그렇지 않으면 브라우저의 CORS 정책에 의해 응답이 거부된다. (인증 정보는 민감한 정보이기 때문에 출처를 정확하게 설정해주어야 한다)

이를 어길 경우 CORS 에러 메시지가 뜬다.

![image.png](image%208.png)

---

# CORS 에러 대응하기

## 서버에서 `Access-Control-Allow-Origin` 응답 헤더 설정하기

서버에서 `Access-Control-Allow-Origin` 헤더를 설정해서 요청을 수락할 출처를 명시적으로 지정 가능. 이 헤더를 세팅하면 출처가 다르더라도 `https://myshop.com`의 리소스 요청을 허용하게 됨

```
'Access-Control-Allow-Origin': <origin> | *
```

- `*`를 설정하면 출처에 상관없이 리소스에 접근할 수 있는 와일드카드이기 때문에 보안에 취약해짐.
- 그래서 `'Access-Control-Allow-Origin': https://myshop.com`과 같이 직접 허용할 출처를 세팅하는 방법이 더 좋음.

## 프락시 서버 사용하기

웹 애플리케이션이 리소스를 직접적으로 요청하는 대신, **프락시 서버**를 사용하여 웹 애플리케이션에서 리소스로의 요청을 전달하는 방법

이 방법을 사용하면, 웹 애플리케이션이 리소스와 동일한 출처에서 요청을 보내는 것처럼 보이므로 CORS 에러를 방지할 수 있음

<aside>

**예시**

---

 `http://example.com`에서 동작하는 웹 애플리케이션이 `http://api.example.com`에 데이터를 요청하는 상황

두 도메인이 다르기 때문에 브라우저는 cross-origin 요청으로 판단하고, 서버가 CORS 설정을 제대로 하지 않았다면 요청이 실패할 수 있음

</aside>

<aside>

**문제 해결 방법**

---

웹 애플리케이션이 직접 `http://api.example.com`에 API 요청하는 대신, **같은 출처(`http://example.com`)에 위치한 프락시 서버**를 통해 API 요청을 중계하도록 구성

```
웹 애플리케이션 → http://example.com/api/proxy → (서버 내부 요청) → http://api.example.com
```

브라우저 입장에선 `http://example.com`에 요청한 것처럼 보이기 때문에, CORS 검사 없이 응답을 받을 수 있음.

</aside>

---

## 참고자료 및 학습자료

### 개인 블로그

https://teddy0.tistory.com/entry/CORS%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-%EA%B7%B8%EB%A7%8C-%EA%B4%B4%EB%A1%AD%ED%98%80

https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-CORS-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%F0%9F%91%8F#cors_%EC%9E%91%EB%8F%99_%EB%B0%A9%EC%8B%9D_3%EA%B0%80%EC%A7%80_%EC%8B%9C%EB%82%98%EB%A6%AC%EC%98%A4

https://evan-moon.github.io/2020/05/21/about-cors/

### 출처

https://docs.tosspayments.com/resources/glossary/cors

https://docs.tosspayments.com/blog/payment-window-cors-error

https://developer.mozilla.org/ko/docs/Web/Security/Defenses/Same-origin_policy

https://developer.mozilla.org/ko/docs/Web/HTTP/Guides/CORS

https://www.holisticseo.digital/technical-seo/http-header/cors/access-control-max-age/