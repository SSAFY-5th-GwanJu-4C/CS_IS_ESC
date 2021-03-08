# OAuth에 대해 알아보자
><br>
>
> 인증과 인가 그리고 OAuth에 대해 알아보자
><br>

#### Reference
- [생활코딩 OAuth2.0](https://opentutorials.org/course/3405)
- [[10분 테코톡] 🎡토니의 인증과 인가](https://youtu.be/y0xMXlOAfss)
- [JWT란?](http://www.opennaru.com/opennaru-blog/jwt-json-web-token/)

---

## 인증과 인가
- __인증__ : 식별가능한 정보로 서비스에 등록된 유저의 신원을 입증하는 과정
- __인가__ : 인증된 사용자에 대한 자원 접근 권한을 확인하는 과정
- 인증은 인가에 선행되며 인증된 유저에 한해서만 접근 권환을 확인한다.

#### 웹에서의 인증과 인가
- 회원만이 게시글을 읽고 쓸 수 있다.
- 작성자만이 본인이 작성한 게시물을 수정, 삭제할 수 있다.

- 예시 
    - 비회원 C가(인증 --> X) 회원 A의 ~~글을 읽는다.~~
    - 회원 B가 본인의 아이디로 로그인(인증 --> OK)해서 회원 A의 글을 읽는다.(인가 --> 가능)
    - 회원 B가 회원 A의 글을 수정한다.(인가 --> 불가능)

#### 정리
- 인증과 인가 : 자원을 적절한/유효한 사용자에게 전달/공개 하기 위한 방법

---

## 인증과 인가의 방법
|단계|방법|
|---|---|
|1. 인증하기|Request Header|
|2. 인증 유지하기|Browser|
|3. 안전하게 인증하기|Server|
|4. 효율적으로 인증하기|Token|
|5. 다른 채널을 통해|__OAuth__|

---

#### 1. Request Header 이용을 통한 인증
```
http://<ID>:<Password>@www.myservice.com/login
```
1. 위와 같이 request header에 ID와 Password값을 넣어 전달.
2. 약속된 방법(Base64 encoding)으로 암호화 된 문자열로 서버는 받아들인다.
3. 서버는 이를 해석하고 DB에서 일치하는 User 정보를 찾는다.
4. 있다면 인증 OK

__단점__ : 매번 인증해야한다.

---

#### 2. Browser를 이용한 인증 유지
- 매번 인증하는 과정을 피하기 위해 브라우저의 스토리지를 이용
- 로컬 스토리지, 세션 스토리지, 쿠키 등으로 ID, PW를 저장.
- `인증된 상태`를 유지

__단점__ : 사용자에게 편한만큼, 해커에게도 해킹하기 편하다.

---

#### 3. Session를 이용한 안전한 인증
- 지금까지는 원하는 자원을 바로 클라이언트로 전달했지만,
- session의 개념이 등장
    1. 사용자가 요청을 보낸다.
    2. 서버는 이를 해석해서 DB에 일치하는 유저가 있는지 확인한다.
    3. 서버는 인증된 사용자의 식별자와 랜덤의 문자열로 세션 ID를 만든다.
    4. 사용자에게 세션 ID를 전달한다.
    5. 사용자는 세션 ID를 저장해서 이용한다.

__장점__ 
1. 크게 위험하지 않다.
2. 세션의 만료기간을 정할 수 있어서 기간이 지나면 해커가 가져가도 위험하지 않다.
3. 서버에서 탈취된 세션을 삭제하면 해당 ID를 이용할 수 없다.

__단점__
- 서비스가 잘되어서 서버를 여러개 두고 로드밸런서를 두었다고 가정해보자.
- 클라이언트에서 첫 요청을해서 세션아이디를 전달 받고 다음 요청을 했을 때,
- 그 이전에 요청을 받았던 서버에만 세션ID가 있는데, 다른 서버에 연결되면
- 접근을 할 수가 없다.

__해결방안__
- 세션DB를 따로둔다.
    - __또 문제점__ : 클라이언트가 많아지면 계속 데이터가 쌓여서 DB가 터진다.

---

#### 4. Token을 이용해서 효율적으로 사용자의 상태를 담아보자.
__아이디어__ : 그러면 요청과 응답 안에, 사용자의 상태를 담아보자 -> __Token__

__JWT(Json Web Token)__
- JSON Web Token의 약자로 전자 서명 된 URL-safe (URL로 이용할 수있는 문자 만 구성된)의 JSON이다.
- 속성 정보 (Claim)를 JSON 데이터 구조로 표현한 토큰으로 RFC7519 표준이다.
- 서버와 클라이언트 간 정보를 주고 받을 때 HTTP 리퀘스트 헤더에 JSON 토큰을 넣은 후 서버는 별도의 인증 과정없이 헤더에 포함되어 있는 JWT 정보를 통해 인증한다.
- 이때 사용되는 JSON 데이터는 URL-Safe 하도록 URL에 포함할 수 있는 문자만으로 만듭니다.
- JWT는 HMAC 알고리즘을 사용하여 비밀키 또는 RSA를 이용한 Public Key / Private Key 쌍으로 서명할 수 있다.
- [더 자세히](http://www.opennaru.com/opennaru-blog/jwt-json-web-token/)

__방법__
1. 사용자가 요청을 보낸다.
2. 서버는 이를 해석해서 DB에 일치하는 유저가 있는지 확인한다.
3. 서버는 Secret Key를 사용해서 JWT를 만들어낸다.
3. JWT(AccessToken이 되고, Key값과 Value값을 가짐)을 응답 헤더 담아 클라이언에게 전달한다.
    - 비밀번호 절대 X
4. 클라이언트는 이 토큰을 저장한다.
5. 클라이언트는 다음부터 요청을 보낼때 헤더에 이 토큰을 담아 보낸다.
6. 각 서버는 본인이 가진 Secret Key로 이 토큰의 유효성 검사를 한다. 
    - 사용자 정보 확인 : 이름, 만료시기, 권한

__장점__
1. 어느 서버에 연결되더라도 각자가 가진 키로 요청을 읽어낸다.
2. 서버가 더 늘어나더라도 Secret key가 있으니, 문제 없이 요청을 읽을 수 있다.
3. 액세스 토큰이 탈취당하더라도 만료기한이 있으므로, 비교적 안전하다.

__단점__
- 유효기간이 지나면 사용자는 계속 AccessToken을 요청해야한다.

#### 해결방안 : RefreshToken
1. 요청을 받으면 AccessToken과 RefreshToken을 동시에 생성한다.
2. 서버는 RefreshToken만 저장하고 AccessToken은 저장하지 않는다.
3. 클라이언트는 두 개의 토큰을 다 저장한다.
4. AccessToken으로 클라이언트는 요청헤더를 보내서 원하는 정보를 얻는다.
5. 시간이 지나서 AccessToken을 담아 요청헤더를 보냈는데, 기간 만료가 뜨면
6. RefreshToken을 담아 요청을 보내서 새로운 AccessToken을 받는다.

#### 아무튼 Token 방식의 핵심
- 토큰으로 상태 관리를 하기에 따로 세션을 둘 필요가 없다.
- 토큰 관리를 해야한다. 결국 토큰도 탈취당할 수 있다.

---
---
## OAuth

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/a8f47411-ea76-478b-99cd-0c345bed9ac6.png "OAuth2 image")

1. 3명의 참가자
    - mine : 나의 서비스
    - User : 사용자
    - Their : 나의 서비스가 연동하려고 하는 그들의 서비스

2. 원하는 동작
    - 사용자가
    - 우리 서비스를 통해서
    - 그들의 서비스에서 동작하는 기능을 이용하고자 한다.
        - ex) 구글캘린더에 우리 서비스 일정 등록
        - ex) 페이스북에 우리 서비스에서 작성한 글을 업로드 등등

3. 그들의 서비스에 접근
    - 사용자로부터 그들의 서비스에서 사용하는 ID와 비밀번호를 받는다. 
        - 우리 서비스를 신뢰하지 못함.
        - 대게 여러 서비스에서 같은 아이디를 사용하기 때문에 더욱 위험함.
        - 우리 입장에서도 유실될 위험이 있어 부담스러움.
    - 이러한 상황에서의 해결책이 바로 __OAuth__

4. OAuth
    1. User의 요청에 의해 그들의 서비스는 AccessToken을 생성한다.
    2. 기능의 전부가 아니라 꼭 필요한 기능을 부분적으로만 접근 허가한다.
    3. 우리의 서비스는 AccessToken을 획득한 다음 그들의 서비스에 접근해서 원하는 작업을 한다.

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/9cc51fe6-770b-4c37-9dc2-943f435b00ee.png "OAuth2 image")

이렇게 생긴 로그인 기능의 가장 기반에 있는 기술이 __OAuth__ 이다.

---

#### 용어 정리


1. 3명의 참가자
    - mine : 나의 서비스
    - User : 사용자
    - Their : 나의 서비스가 연동하려고 하는 그들의 서비스

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/fd4f24ea-bc46-42e3-ab4f-2eff2c96df20.png "OAuth2 image")

2. 3명의 참가자
    - Client(mine) : 리소스 서버에 접속해서 정보를 가져가는 클라이언트(mine)
    - Resource Owner(User) : 자원의 소유자(User)
    - Resource Server(Their) : 우리가 제공하고자 하는 자원을 가지고 있는 서버(Thier), 데이터를 가지고 있는 서버
    - Authorization Server(Their+) : 인증과 관련된 처리를 전담하는 서버

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/4cb395ed-468e-4805-9f60-e2bed447e8a3.png "OAuth2 image")

---
## OAuth 활용의 단계
|단계|
|---|
|1. Client를 Resource Server에 등록|
|2. Resource Owner의 승인|
|3. Resource Server의 승인|
|4. Access Token 발급|
|5. API 호출|
|6. Refresh Token|

---
#### 1. Client를 Resource Server에 등록

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/95cb3c1c-fc96-46db-9f29-aa05ef4e1164.png "OAuth2 image")

클라이언트가 리소스서버를 이용하기 위해서는 리소스 서버의 승인을 사전에 받아놓아야 한다.
등록 방법은 서비스마다 다르지만

공통적인 부분이 있다.
![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/cb2aa606-c14c-4782-a1b0-3be38b4e9d90.png "OAuth2 image")

- Client ID : 우리의 서버를 식별하는 식별자
- Secret : Clinent ID의 비밀번호
- Authorized redirect URIs : 리소스 서버는 권한을 부여하는 과정에서 우리 서비스에게 `Authoriized code`라는 값을 제공한다. 그 값을 전달할 주소가 `Authorized redirect URIs`이다.

Client를 Resource Server에 등록하는 것은
Resource Server에 Client가 본인을 알리고, Client ID와 Secret을 받아오는 것을 의미한다.

<details>
<summary>FaceBook 예시(2년 전 ver)</summary>
<div markdown="1">

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/41b3d6b2-ae05-4bc8-904c-05009aa7d56e.png "OAuth2 image")

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/6762ac48-32f4-4e63-9d65-5a936aa3d4ed.png "OAuth2 image")

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/4da3aacc-77b2-4d46-a53c-d5e8d771fb34.png "OAuth2 image")

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/2e3eeff5-7011-4d59-95d9-dd778a644687.png "OAuth2 image")

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/995a9144-acf2-4c7e-8ec5-8d71f655f0c6.png "OAuth2 image")

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/3f3a17fa-971e-4fe0-94c4-352c0a68420a.png "OAuth2 image")

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/2e08198a-d46e-4b01-a00d-eccb29e21fc0.png "OAuth2 image")

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/89ceb5e0-85bc-4ff7-b98b-a2e2618bfbad.png "OAuth2 image")

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/850b3237-8526-4399-b57a-3cef292ecd00.png "OAuth2 image")

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/496c2a49-9d7a-4a40-923f-ef8c25240985.png "OAuth2 image")

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/f7ad5952-dfb5-40fa-8146-5a35dc85b1b1.png "OAuth2 image")

</div>
</details>

<details>
<summary>Google 예시(작년)</summary>
<div markdown="1">
<br>
<a href="https://daily-mulgyeol.tistory.com/43)"> Django - social login </a>
</div>
</details>

---

#### 2. Resource Owner의 승인


![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/dbbe8fe9-990d-4cd0-a032-666b17121856.png "OAuth2 image")

기능이 A,B,C,D라고 했을 때 필요한 기능에 대해서만 접근을 허가하는 것이 모두에게 좋다.


아무튼, 
Resource Server는 client id와 secret을 발급했고 redirect URL을 가지고 있다.
Client는 client id와 secret을 가지고 있다.
Resource Owner가 Client를 통해 Resource Server의 기능을 이용하고 싶다.


Resource Owner가 처음 Client에 요청을 보내면,
![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/14bf1fb5-dd8f-4d60-8af4-85415bbdd242.png "OAuth2 image")

Client 위의 화면을 보여준다.
"당신이 이 기능을 이용하기 위해서는 `인증`과정을 거쳐야 한다" -> 로그인 해라.
이 버튼을 클릭하는 것은 "그것을 동의한다"라는 의미이다.
이 버튼은

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/7624496a-8b20-431d-bba2-c5d337beadc4.png "OAuth2 image")

클라이언트가 제공한 그림에서 생성된 주소이다.
저 주소로 접속하면
Resource Server는 Resource Owner가 로그인을 했는지 확인한다.
로그인이 안되어있으면

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/e33192b3-370d-4460-b3da-ad90c6876adb.png "OAuth2 image")


로그인을 하라는 창을 띄우주고,
로그인을 성공하면 그때서야 cliend id값과 같은 clent id가 가지고잇는지 확인하다.
그 id가 있다면 redirect url이 서로 같은지 확인한다.
다르면 끊어버리고,
같다면 Scope에 해당하는 기능에 접근하는 권한을 Client에게 부여할 것인지를 확인하는 메시지를
Resource Owner에게 전달한다.

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/9a6c160a-9f75-4195-a918-c2247a585cbe.png "OAuth2 image")

허용 버튼을 누르면 resource 서버로 허용했다는 정보를 전달하고,
허락한 user id와 scope정보를 server가 저장한다.

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/f359fd9d-6dfb-44e2-a7fd-42959a773690.png "OAuth2 image")

---
#### 3.Resource Server의 승인

현재는 Resource Owner에게 승인을 받은상태이고,
그러나 Server는 바로 accessToken 을 발급하지 않고
한 단계 절차가 더 거친다.

__Authorization code__

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/341b8c34-9b93-441e-937f-85eef9468dba.png "OAuth2 image")


이런식으로 Authorization Code를 owner에게 전달한다.
응답할 때 header값으로 Location을 주면 redirection이라고 한다.

아무튼, 응답은
Resource Server가 Resource Owner의 웹 브라우저에게, 저 주소로 이동해!라고 명령한 것이다.
이 주소를 받으면 Resource Owner의 브라우저는 Owner가 인식하지도 못하게
이 주소로 이동한다.

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/ec27e75b-761e-4028-bb11-589bd469ca94.png "OAuth2 image")


이 과정을 통해 클라이언트는 authorization code를 알게된다.

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/9dc4282e-0cea-44f0-b72b-750a463ebdfd.png "OAuth2 image")


자, 클라이언트는 이제 리소스 오너를 통하지않고 아래와 같은 주소로 리소스 서버에게 직접 접근한다.

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/630c4e04-16f3-42af-baf0-02ebeb2ca9a4.png "OAuth2 image")


client_secret값도 함께 전송한다. (중요)





이제 리소스 서버는
authorization code를 확인해서,
자기가 가지고 있는 authorization code와 비교하여 같은 것이 있는지 확인한다.
같은 것이 았다면 client id와 client secret, 그리고 redirect URL을 비교한다.


일치하면

---
#### 4.Access Token 발급

- OAuth의 목적 : Access Token을 발급하는 것.


resource server는 이제 authorization code값으로 확인을 했기 때문에,
authorization code값을 지운다.

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/2005ba08-43d9-49d7-8031-2c7d3680d7b1.png "OAuth2 image")

이렇게,
드디어 리소스 서버는 AccessToken을 발급한다.
그리고 클라이언트에게 전달한다.

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/0b0c6b81-7f3e-40ef-a328-78a75924bd9c.png "OAuth2 image")


클라이언트는 AccessToken을 내부적으로 저장한다.

클라이언트가 AccessToken으로 접근을 하게 되면,
Resource Server는 AccessToken을 확인하고,
UserID에 해당하는 유저에 대하여, b,c 기능을 이 AccessToken을 가진 Client에게 허가한다.

---
#### 5. API호출

이제 AccessToken을 활용해서 Resource Server를 핸들링 해야한다.
조작을 하기 위해서는 `Resource Server`가 클라이언트들에게
우리를 쓰려면 이렇게 이렇게 해야한다.

그 방식을 API(Application Programming Interface)라고 한다.

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/d3b96aab-2118-48be-bfbb-ae9620cb45d3.png "OAuth2 image")



Google API사용


자 어쨌든 Access Token을 얻었다고 치고, 대략적인 API 호출 방법을 알아볼 사람은
펼쳐보자.

<details>
<summary>API 호출방법</summary>
<div markdown="1">

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/28915d0d-cd05-401b-a074-705f056bfd28.png "OAuth2 image")

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/4499b8c3-0983-465b-a70c-4aa5efaa8be1.png "OAuth2 image")


1. access_token을 query parameter로 전송하는 방법


2. Authorization : bearer라고 하는 것을 HTTP header로 전송하는 방법

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/2b39ba14-fb4a-4d8e-b974-e6319271afe4.png "OAuth2 image")





1. 첫번째 방법해보자.

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/3e9d4a3e-943d-4963-871b-73d6005d1924.png "OAuth2 image")


위의 값은 캘린더리스트에 접근하는 api, 지금들어가면, 요렇게!

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/91c20a52-60cc-4313-9316-57d88bbd867c.png "OAuth2 image")


밑은 accessToken





`access_token =`을 넣고


밑의 accessToken을 복붙




![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/840140d4-a54b-4c5a-a184-3c66665a3ca7.png "OAuth2 image")





이렇게 접속을 하면!

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/6a621ffa-2ea1-4686-a16a-fab8ca1dba17.png "OAuth2 image")


내용이 나온다.








2. 두 번째 방법

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/38688e9b-c72e-4ace-9b0d-916a94e1674d.png "OAuth2 image")


헤더 값으로 전송

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/d6ebea13-6570-4f0f-83cb-53e3e6a6d8b5.png "OAuth2 image")


curl이라는 프로그램을 사용하면 손쉽게 이용 가능하다.


-H는 옵션

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/6b32327f-751d-4771-85fb-5ea68757f3e1.png "OAuth2 image")


음영 부분이 헤더값!


그 뒤에다가 api의 주소를 입력







![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/2aa2e2f5-633e-435e-8bd2-90d4d49132e0.png "OAuth2 image")


엑세스 토큰을 음영 부분에 넣어주기

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/c855a01d-c271-45e0-8f07-b73872a030a4.png "OAuth2 image")


api의 주소값을 뒷부분에 넣어주기

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/34fb8dc1-b9ea-478c-aa04-673694309827.png "OAuth2 image")

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/db85ad12-c010-4a74-81b5-2fe80243646d.png "OAuth2 image")





curl은 주소를 입력하면 웹페이지를 다운받는 프로그램.

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/56e4a1c7-7940-448b-aaf9-1b0e64f17983.png "OAuth2 image")

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/99961be7-8d8e-4c2c-a64c-ea7478d0a87c.png "OAuth2 image")

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/f681c6f6-8ad5-4c36-bb07-dd430c82a4c0.png "OAuth2 image")

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/8f16d39c-a685-479f-a0ac-b626b72a9a91.png "OAuth2 image")


이렇게 하면 서버랑 좀 더 안전하게 서버와 통신이 가능하다.

1번 방식은 시스템에서 제공을 할 수도 있고 안할 수도 있다.
2번째 방식은 표준화된 방식이라 어디서나 사용가능하고 안전한 방식이다.

</div>
</details>


---
#### 6. Refresh Token

- [RFC6749](https://tools.ietf.org/html/rfc6749#section-1.5)
- 위에서 알아본 Refresh Token까지 활용하는 OAuth 인증 과정을 살펴보자.

```
  +--------+                                           +---------------+
  |        |--(A)------- Authorization Grant --------->|               |
  |        |                                           |               |
  |        |<-(B)----------- Access Token -------------|               |
  |        |               & Refresh Token             |               |
  |        |                                           |               |
  |        |                            +----------+   |               |
  |        |--(C)---- Access Token ---->|          |   |               |
  |        |                            |          |   |               |
  |        |<-(D)- Protected Resource --| Resource |   | Authorization |
  | Client |                            |  Server  |   |     Server    |
  |        |--(E)---- Access Token ---->|          |   |               |
  |        |                            |          |   |               |
  |        |<-(F)- Invalid Token Error -|          |   |               |
  |        |                            +----------+   |               |
  |        |                                           |               |
  |        |--(G)----------- Refresh Token ----------->|               |
  |        |                                           |               |
  |        |<-(H)----------- Access Token -------------|               |
  +--------+           & Optional Refresh Token        +---------------+
```

- (A) : 권한을 획득한다.
- (B) : AccessToken을 발급한다, RefreshToken도 발급한다.
- (C) : API를 호출할때는 AccessToken을 제출한다.
- (D) : 보호되고 있는 자원을 획득한다.
---------- 시간의 경과 ----------

- (E) : AccessToken을 제출한다.
- (F) : 유효하지 않다는 에러가 뜬다.
- (G) : RefreshToken을 제출한다.
- (H) : AccessToken을을 재발급 받는다. RefreshToken을 갱신하는 경우도 있다.

<details>
<summary>구글의 Refreshing 메뉴얼</summary>
<div markdown="1">

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/14d2fd2d-fd83-4068-99ee-4e441a048981.png "OAuth2 image")

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/99be47a0-de0e-44f6-854e-1bdecc227fe7.png "OAuth2 image")

![](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/capture_images/dcc70d9291b44c70a992f6bd25528a81/8e03e526-64fa-447e-ad26-5a2d69e8852c.png "OAuth2 image")


새로 발급된 액세스 토큰과
얼마동안 유효한지.
표준화 된 방법이라 대부분 이렇게 사용함.

</div>
</details>

---
#끝