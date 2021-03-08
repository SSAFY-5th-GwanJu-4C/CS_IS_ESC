# CORS란?

#### Reference
- [CORS는 왜 이렇게 우리를 힘들게 하는걸까?](https://evan-moon.github.io/2020/05/21/about-cors/)
- [교차 출처 리소스 공유(CORS)](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS#%EC%A0%91%EA%B7%BC_%EC%A0%9C%EC%96%B4_%EC%8B%9C%EB%82%98%EB%A6%AC%EC%98%A4_%EC%98%88%EC%A0%9C))
- [What is a URL?](https://developer.mozilla.org/ko/docs/Learn/Common_questions/What_is_a_URL8)
- [XSS와 CSRF에 대하여](https://glasgowkiss.tistory.com/16)


---

## CORS는 뭔가요?
- `Cross-Origin Resource Sharing(CORS)`은 추가적인 HTTP header를 사용해서 애플리케이션이 다른 origin의 리소스에 접근할 수 있도록 하는 메커니즘이다.
- 다른 origin에서 내 리소스에 함부로 접근하지 못하게 하기 위해서도 사용된다.
- 브라우저간의 데이터를 주고받는 과정에서, 도메인 이름이 서로 다른 사이트간에 api요청을 할 때, 공유를 설정하지 않았다면 CORS에러가 발생한다.

---
## Origin(출처)는 뭔가요?

- 출처는 URL의 구성요소에서 `Protocol`과 `Host`, 그리고 `:80`나 `:443`과 같은 포트 번호까지 모두 합친 것을 말한다. 
- 다시 말해, 서버의 위치를 찾아가기 위해 필요한 가장 기본적인 것들을 합쳐놓은 것이다.

#### URL구성요소
> `http://www.example.com:80/path/to/myfile.html?key1=value1&key2=value2#SomewhereInTheDocument`
- Protocol : `http://`
- host(Domain Name) : `www.example.com`
- Port : `:80`
    - 포트는 기술적으로 웹서버에서 자원을 접근하기 위해 사용하는 `관문(gate)`를 가리킨다.
    - 만약 웹서버가 자원에 접근 하기 위해 표준  HTTP 포트(HTTP :80, HTTPS :443)를 사용한다면, 포트 번호는 보통 생략한다.
    - 그렇지 않으면 포트 번호는 필수이다.
- Path : `/path/to/myfile.html`
    - 웹서버에서 자원에 대한 경로이다.
    - 초기에는 웹 서버상에서 물리적 파일 위치를 나타냈다.
    - 최근에는 실제 물리적 경로를 나타내지않고, 웹 서버에서 추상화해서 보여준다.
- Parameters, (Querystring) : `?key1=value1&key2=value2`
    - Querystring : 사용자가 입력 데이터를 전달하는 방법중의 하나로, url 주소에 미리 협의된 데이터를 파라미터를 통해 넘기는 것을 말한다.
    - `?key1=value1&key2=value2` 는 웹서버에 제공하는 추가 파라미터이다. 
    - 이 파라미터들은 & 기호로 구분된 키/값으로 짝을 이룬 리스트이다.
    - 웹 서버는 자원을 반환하기 전에 추가적인 작업을 위해 이런 파라미터들을 사용할 수 있다. 
    - 각각의 웹서버는 파라미터들을 언급하는 자신의 규칙을 갖는다. 
    - 특정한 웹서버가 파라미터를 다루는 지 알기 위한 유일한 방법은 웹서버 소유자에게 묻는 것이다.
- Fragment, Anchor : `#SomewhereInTheDocument`
    - `#SomewhereInTheDocument`는 자원 자체의 다른 부분에 대한 `anchor(닻)`이다.
    - `앵커는` 는 일종의 자원 안에서 "bookmark" 이다. 
        - 즉, "bookmarked" 지점에 위치된 내용을 보여주기 위해 브라우저에게 방향을 알려준다.
        - 예를 들어, HTML 문서에서 브라우저는 anchor가 정의한 곳의 점을 스크롤한다.

---

## SOP(Same-Origin Policy)
- "같은 출처에서만 리소스를 공유할 수 있다"라는 규칙을 가진 정책이다.
- 웹은 오픈스페이스 환경이므로 다른 출처에 있는 리소스를 가져와서 사용하는 일이 굉장히 흔하다. 따라서, 다른 출처의 리소스에 접근하는 것을 모조리 막을 수만은 없으니, 몇가지 예외 조항을 두어 리소스 요청을 허용한다.
- 그 중 하나가 "CORS 정책을 지킨 리소스 요청"이다.
- 즉, 다른 출처로 리소스를 요청한다면 SOP 정책을 위반한 것이고, SOP의 예외 조항인 CORS 정책까지 지키지 않는다면 아예 다른 출처의 리소스를 사용할 수 없다.

---
## 왜 이런 정책을 만들었는가?
- 두 애플리 케이션이 제약 없이 소통할 수 있는 환경은 위험하다.
- 예를 들어 다른 출처의 어플리케이션이 서로 통신하는 것에 대해 아무런 제약도 존재하지 않는다면, 악의를 가진 사용자가 소스 코드를 살핀 후 `CSRF(Cross-Site Request Forgery)`나 `XSS(Cross-Site Scripting)`와 같은 방법을 사용하여 여러분의 어플리케이션에서 코드가 실행된 것처럼 꾸며서 사용자의 정보를 탈취하기 쉽다.

#### CSRF(Cross Site Request Forgery)
- CSRF 공격(Cross Site Request Forgery)은 웹 어플리케이션 취약점 중 하나로 인터넷 사용자(희생자)가 자신의 의지와는 무관하게 공격자가 의도한 행위(수정, 삭제, 등록 등)를 특정 웹사이트에 요청하게 만드는 공격이다.
- XSS를 이용한 공격이 사용자가 특정 웹사이트를 신용하는 점을 노린 것이라면, 사이트간 요청 위조는 특정 웹사이트가 사용자의 웹 브라우저를 신용하는 상태를 노린 것이다.
- 일단 사용자가 웹사이트에 로그인한 상태에서 사이트간 요청 위조 공격 코드가 삽입된 페이지를 열면, 공격 대상이 되는 웹사이트는 위조된 공격 명령이 믿을 수 있는 사용자로부터 발송된 것으로 판단하게 되어 공격에 노출된다.



#### XSS(Cross Site Scripting)
- 웹 애플리케이션에서 많이 나타나는 취약점의 하나로, 웹사이트 관리자가 아닌 이가 웹 페이지에 악성 스크립트를 삽입할 수 있는 취약점이다.
- 웹 애플리케이션이 사용자로부터 입력 받은 값을 제대로 검사하지 않고 사용할 경우 나타난다. 주로 여러 사용자가 보게 되는 전자 게시판에 악성 스크립트가 담긴 글을 올리는 형태로 이루어진다.
- 해커가 사용자의 정보(쿠키, 세션 등)를 탈취하거나, 자동으로 비정상적인 기능을 수행하게 하거나 할 수 있다. 주로 다른 웹사이트와 정보를 교환하는 식으로 작동한다.

#### XSS와 CSRF의 차이요약
- XSS는 공격대상이 Client이고, CSRF는 Server이다.
- XSS는 사이트변조나 백도어를 통해 클라이언트에 대한 악성공격을 한다.
- CSRF는 요청을 위조하여 사용자의 권한을 이용해 서버에 대한 악성공격을 한다.

---
## 같은 출처와 다른 출처의 구분
- 두 RUL의 구성 요소중 `Scheme`,`Host`,`Port` 3가지만 동일하면 `같은 출처`이다.
- IE를 제외하면 대부분 위의 형식을 따르므로, 그냥 이렇게 알아두자.

---
## 출처를 비교하는 로직은 브라우저에 구현되어 있다.
- CORS 정책을 위반하는 리소스 요청을 할 경우
    1. 리소스 요청을 한다.
    2. 서버는 정상적으로 응답을 한다.
    3. 브라우저는 이 응답을 분석해서 CORS 정책 위반이라고 판단되면 그 응답을 사용하지 않고 버린다.
- 결론적으로 브라우저를 통하지 않고 서버간 통신을 할 때는 이 정책이 적용되지 않는다.
- CORS 정책 위반 에러가 발생 했다고 해도 서버 쪽 로그에는 정상적으로 응답했다는 로그만 남는다.

---
## CORS 동작 방법

#### 기본동작, `Origin Field`와 `Access-Control-Allow-Origin`
- 기본적으로 웹 클라이언트 어플리케이션이 다른 출처의 리소스를 요청할 때, 브라우저는 HTTP 프로토콜 요청 헤더의 Origin 필드에 요청을 보내는 출처를 함께 당아 보낸다.
- 이후 서버가 응답할 때, 헤더의 Access-Control-Allow-Origin라는 값에 "이 리소스를 접근하는 것이 허용된 출처"값을 내려준다.
- 브라우저는 자신이 보낸 Origin과 응답의 Access-Control-Allow-Origin를 비교한다.
- 기본적인 흐름은 간단하지만, CORS가 동작하는 방식은 세 가지 시나리오에 따라 변경된다.

---

#### 1. PreFlight Request
<p align="center"><img src="./img/CORS/cors-preflight.png" width = "800px"></p><br>

- 일반적으로 웹 애플리케이션을 개발할 때 마주치는 시나리오.
- 브라우저는 요청을 한번에 보내지 않고 예비 요청과 본 요청으로 나누어서 서버로 전송한다.
    - 브라우저가 본 요청에 보내기 전에 보내는 예비 요청을 `Preflight` 라고 부른다.
    - `Preflight`에는 HTTP 메소드 중 `OPTIONS` 메소드가 사용된다.
    ` 본 요청을 보내기 전에 브라우저 스스로 이 요청을 보내는 것이 안전한지 확인한다.
- __과정__
    1. 브라우저에게 리소스를 받아오라는 명령을 내린다.
    2. 브라우저는 서버에게 예비 요청을 먼저 보낸다.
    3. 서버는 이 예비 요청에 대한 응답으로 현재 자신이 어떤 것들을 허용하고, 어떤 것들을 금지하고 있는지에 대한 정보를 응답 헤더에 담아서 브라우저에게 다시 보낸다.
    4. 이후 브라우저는 자신이 보낸 예비 요청과 서버가 응답에 담아준 허용 정책을 비교한다.
    5. 이 요청을 보내는 것이 안전하다고 판단되면 같은 엔드포인트로 다시 본 요청을 보낸다.
        - 만약 출처가 달라 안전하지 않다고 판단되면 CORS 정책을 위반했다는 에러 메시지를 띄운다.
    6. 이후 서버가 이 본 요청에 대한 응답을 한다.

---

#### 2. Simple Request
<p align="center"><img src="./img/CORS/cors-preflight.png" width = "800px"></p><br>

- 정식 명칭은 없지만 [MDN의 CORS 문서](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS#%EC%A0%91%EA%B7%BC_%EC%A0%9C%EC%96%B4_%EC%8B%9C%EB%82%98%EB%A6%AC%EC%98%A4_%EC%98%88%EC%A0%9C)에서 이 시나리오를 Simple Request라고 부른다.
- __과정__
    1. 단순 요청은 예비 요청을 보내지 않고 바로 서버에게 본 요청을 보낸다.
    2. 서버가 이에 대한 응답 해더에 `Access-Control-Allow-Origin`값을 보내준다.
    3. 브라우저가 CORS 정책 위반 여부를 검사한다.
- 특정 조건을 만족하는 경우에만 예비 욫어을 사용하는 단순 요청을 사용할 수 있다.(꽤나 복잡하다.)
    1. 요청의 메소드는 GET, HEAD, POST 중 하나여야 한다.
    2. Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width, Width를 제외한 헤더를 사용하면 안된다.
    3. 만약 Content-Type를 사용하는 경우에는 application/x-www-form-urlencoded, multipart/form-data, text/plain만 허용된다.

---
#### 3. Credentialed Request

- 인증된 요청을 사용하여 다른 출처간 통신에서 좀 더 보안을 강화하고 싶을 때 사용한다.
- 기본적으로 브라우저가 제공하는 비동기 리소스 요청 API인 XMLHttpRequest 객체나 fetch API는 별도의 옵션 없이 브라우저의 쿠키 정보나 인증과 관련된 헤더를 함부로 요청에 담지 않는다. 
- 이때 요청에 인증과 관련된 정보를 담을 수 있게 해주는 옵션이 바로 `credentials` 옵션이다.
    - `credentials` 옵션에서는 총 3가지 값을 사용할 수 잇으며, 각 값들의 의미는 아래와 같다.
    - 옵션 값 |	설명|
      ---|---|
      same-origin (기본값)|	같은 출처 간 요청에만 인증 정보를 담을 수 있다|
      include|	모든 요청에 인증 정보를 담을 수 있다|
      omit|	모든 요청에 인증 정보를 담지 않는다|
- `Access-Control-Allow-Origin`값이 `*` 이고, `credentials: include` 옵션값을 사용한 경우.
    - 해석 : 모든 출처를 허용하고, 동일 출처 여부와 상관없이 무조건 요청에 인증 정보가 포함되도록 설정했다.
    - 결과 : 
        - 인증 모드가 `include`일 경우, 모든 요청을 허용한다는 의미의 `*`를 Access-Control-Allow-Origin 헤더에 사용하면 안된다는 에러메시지 발생한다.
        - 요청에 인증 정보가 담겨져 있는 상태에서 다른 출처의 리소스를 요청하게 되면 브라우저는 CORS 정책 위반 여부를 검사하는 룰에 두 가지를 추가한다.
            1. `Access-Control-Allow-Origin`에는 `*`를 사용할 수 없고, 명시적인 URL 을 사용해야한다.
            2. 응답헤더에는 반드시 `Access-Control-Allow-Credentials: true`가 존재해야한다.

---
## 결론
- 백엔드 개발자는 서버 애플리케이션의 응답 헤더에 올바른 `Access-Control-Allow-Origin`이 내려올 수 있도록 세팅해줘야 한다.
