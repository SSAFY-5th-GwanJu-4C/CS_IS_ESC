# 1. CORS란?

- CORS란 `Cross-Origin-Resource-Sharing` 의 약자로 "추가적인 HTTP Header를 통해 웹 애플리케이션이 다른 **출처**의 자원에 접근할 수 있는 권한을 부여하는 **웹 브라우저**의 자원 관리 정책"

    → 서버에 다른 출처의 요청을 거절하는 로직이 작성되어 있지 않다면, 서버는 웹 클라이언트의 요청에 정상적으로 응답하지만, 브라우저 상에서 CORS에 위반되면 해당 응답을 버림

    → 서버에서는 `200 OK`로 응답했지만 웹 애플리케이션에서는 CORS Error가 발생할 수 있음 

    🙋‍♂️ 출처가 무엇인가요?

    📢 우리가 흔히 사용하는 URL에서 **Protocol, Host, Port Number**를 출처라고 칭함

---

# 2. Why CORS?

- 보안상의 문제
    - 웹 애플리케이션의 경우 `개발자 모드`만 이용하더라도 DOM, 자원의 출처 등 다양한 정보를 제약없이 열람할 수 있기 때문에 자원을 사용하는 것에 대한 보안상의 제약이 필요함

        🔎 SOP(Same Origin Policy) - 웹 생태계에서 CORS와 함께 사용되는 자원 관리 정책 중 하나로, "같은 출처의 자원만 공유할 수 있다."는 제약

        → 하지만 웹 환경에서 다른 출처의 자원을 사용하지 않는 것은 사실상 불가능하기 때문에 몇가지 예외 사항을 두고 다른 출처의 자원을 허용하였는데, 그 중 하나가 CORS

---

# 3. CORS 동작 방식

1. 웹 클라이언트가 다른 출처의 자원을 요청하기 위해 HTTP Header에 `Origin` 영역에 자신의 출처를 작성하여 HTTP Request를 전송함

    ```java
    Request
    GET https://server.com/resource

    Origin: https://host.com
    ...
    ```

2. 서버는 다른 출처에서 자신의 출처의 자원을 사용하도록 허락한다면 HTTP Response의 `Access-Control-Alllow-Origin` 영역에 `Origin`으로 넘겨받은 클라이언트의 출처를 담아 전송

    ```java
    Response
    GET https://server.com/resource 200 OK

    Access-Control-Allow-Origin: https://host.com
    ...
    ```

3. 이를 전달받은 웹 브라우저는 자신이 보낸 `Origin` 영역과 전달받은 `Access-Control-Allow-Origin` 영역을 비교하여 유효하다면 해당 자원을 사용

---

## 3.1) Preflight Request

- 가장 많이 접하는 시나리오
- 웹 브라우저가 HTTP Request를 전송하기 이전에 HTTP `OPTIONS` 메소드를 이용하여 예비 요청(Preflight Request)를 전송
- 서버는 자신의 `Access-Control-Allow-Origin` 필드와 여러가지 정보를 담아 HTTP Response 전송
- 브라우저는 자신의 Preflight Request의 출처와 서버의 HTTP Reponse의 `Access-Control-Allow-Origin` 영역을 비교하여 CORS 여부 판단

---

## 3.2) Simple Request

- Preflight Request과정 없이 바로 HTTP Request를 보내는 시나리오
- Simple Request를 하기 위해서는 특정 조건들이 충족되어야 함
    - HTTP Request 메소드는 `GET` , `HEAD` , `POST` 중 하나여야 함
    - `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, `DPR`, `Downlink`, `Save-Data`, `Viewport-Width`, `Width`를 제외한 헤더를 사용하면 안됨
    - 만약 `Content-Type`를 사용하는 경우 `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain`만 허용

---

## 3.3) Credentialed Request

- CORS의 보안을 강화하여 HTTP Request를 보내는 시나리오
- 일반적으로 `XMLHttpRequest`와 `Fetch API`는 별도의 요청이 없으면 브라우저의 쿠키나 인증 관련 정보를 HTTP Header에 담지 않음
    - `credential` : 인증 관련 정보를 HTTP Header에 담을 수 있도록 해주는 옵션
        - **same-origin(**default) : 같은 출처 간 인증 정보를 담을 수 있음
        - **include** : 모든 요청에 인증 정보를 담을 수 있음
        - **omit** : 모든 요청에 인증 정보를 담지 않음
- credential에 same-origin이나 include 옵션을 부여하여 HTTP Request를 보낼 경우 브라우저에서 추가적인 정책으로 CORS 판단
    - `Access-Control-Allow-Origin` 필드에 와일드카드 `*`를 사용할 수 없으며, 정확한 URL을 명시해야 함
    - 서버의 HTTP Response Header에는 `Access-Control-Allow-Credentials: true` 가 존재해야 함

---

[출처]

[https://evan-moon.github.io/2020/05/21/about-cors/](https://evan-moon.github.io/2020/05/21/about-cors/)

[https://developer.mozilla.org/ko/docs/Web/HTTP/CORS](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)
