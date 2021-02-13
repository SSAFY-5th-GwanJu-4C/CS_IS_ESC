# REST, REST API, RESTful API
><br>
>
> RestfulAPI에 대해 알아보자
><br>

#### Reference
- [REST API가 뭔가요?](https://youtu.be/iOueE9AXDQQ)
- [그런 REST API로 괜찮은가](https://youtu.be/RP_f5dMoHFc)
- [[Network] REST란? REST API란? RESTful이란?](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)

# 일반적으로 개발자들이 이해하고 사용하는 Restful API

## RESTful API
- 정보들이 주고받아 지는 데에 있어서 개발자들 사이에 널리 쓰이는 형식

---

## API(Application Programming Interface) 
- 소프트웨어가 다른 소프트웨어로부터 지정된 형식으로 요청, 명령을 받을 수 있는 수단

---

## 다시, RESTful
- 프론트엔드 웹에서 서버에 데이터를 요청하거나 배달 앱에서 서버에 주문을 넣는 등의 서비스에서 오늘날 많이 사용되는 방식이 REST란 형식의 API이다.
- RESTful은 일반적으로 REST라는 아키텍처를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어이다.
- ‘REST API’를 제공하는 웹 서비스를 ‘RESTful’하다고 할 수 있다.
- RESTful은 REST를 REST답게 쓰기 위한 방법으로, 누군가가 공식적으로 발표한 것이 아니다.
- 즉, REST 원리를 따르는 시스템은 RESTful이란 용어로 지칭된다.

---

## RESTful의 목적
- 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것
- RESTful한 API를 구현하는 근본적인 목적이 성능 향상에 있는 것이 아니라 일관적인 컨벤션을 통한 API의 이해도 및 호환성을 높이는 것이 주 동기이니, 성능이 중요한 상황에서는 굳이 RESTful한 API를 구현할 필요는 없다.

--- 

## RESTful하지 않은 API - 1
1. CRUD 기능을 모두 POST로만 처리하는 API
2. route에 resource, id 외의 정보가 들어가는 경우(/students/updateName)

---

## RESTful API의 가장 중요한 특징
- 각 요청이 어떤 동작이나 정보를 위한 것인지를 그 요청의 모습 자체로 추론이 가능하다.
- 목적에서 언급 했듯이, 기능 자체만을 중요하게 생각한다면 동작만 되게끔 짜면 그만이다.

---

## RESTful하지 않은 API - 2

예를 들어
학교의 데이터베이스로부터 정보를 요청한다고 생각해보자.
1. `https://(도메인)/1` 은 `반의 리스트`를 요청
2. `https://(도메인)/2` 는 `반의 학생들의 리스트`를 요청
3. `https://(도메인)/3` 은 `학생 정보 수정` 요청

위와 같이 설정할 경우, 이에 맞춰 요청을 보내도록 하기만 하면 사실 동작하는데 아무런 문제가 없다.

문제는 서비스를 이 개발자 혼자 만드는게 아니라는 것이다. 이 일을 인계받을 개발자나 이 API를 사용해서 다른 제품을 만들 개발자들은 굉장히 일하기 힘들어질 것이다.

---

## RESTful API

RESTful하게 만든 API는 요청을 보내는 주소만으로도 대략 이게 뭘하는 요청인지 파악이 가능하다.

다시, 학교 데이터베이스로부터 정보를 요청할 때,
1. `https://(도메인)/classes`는 `반의 리스트`를 요청
2. `https://(도메인)/classes/1` 은 `1반의 정보` 요청
3. `https://(도메인)/classes/1/students` 는 `1반의 학생들 정보` 요청
4. `https://(도메인)/classes/1/students/13` 은 `1반의 13번 학생의 정보` 요청
5. `https://(도메인)/classes/1/students?grade=A` 은 `1반의 학생들 중 등급이 A인 학생들 정보` 요청
6. `https://(도메인)/classes/1/students?page=2&count=10` 는 `한 페이지에 10명씩 정보를 받아오는 요청`의 예

---

### URI

> 6. `https://(도메인)/classes/1/students?page=2&count=10` 

- 위와 같이 자원을 구조와 함께 나타내는 형태의 구분자를 URI라고 한다.
- [URI & URL](https://velog.io/@jch9537/URI-URL)


---

## HTTP method

>서버에 REST API로 요청할 때는 HTTP란 규약에 따라 요청을 전송한다.
> HTTP로 요청을 보낼때는 GET, POST, DELETE, PUT 4가지나, PATCH를 더해 5가지 방식을 사용한다.

- POST, PUT, PATCH에는 body란 주머니가 있어서 정보를 GET이나 DELETE 보다 많이, 그리고 비교적 안전하게 감춰서 실어보낼 수 있다.
- 사실 이것들의 기능이 특정 용도에 제한되어있지 않다.
    - __POST만으로도 CRUD가 가능하다.__
- 하지만 누구든 각 요청의 의도를 쉽게 파악할 수 있도록, RESTful하게 API를 만들기 위해서는 이들을 목적에 따라 구분해서 사용해야한다.

---
## GET

- GET : 데이터를 Read, 조회하는데 사용한다.
- `[GET] https://(도메인)/classes/1/students` 이라는 요청이 있다면 개발자들은 1반의 학생들을 보는 요청이구나 하고 짐작할 수 있다.

--- 
## POST

- POST : 데이터를 Create, 새로운 정보를 추가하는데 사용된다.
1반에 새 학생이 들어와서 정보를 추가하려면 
- `[POST] https://(도메인)/classes/1/students`라는 요청을 전송해서 BODY에 새 학생의 정보를 실어보낸다.
- 이 때, 학생 번호는 추가되면서 자동으로 생성되게 할 수 있고, 이름이나 성별, 등급은 body에 담아 보낸다.

---
## PUT & PATCH

- PUT : 데이터를 Update, 정보를 변경하는데 사용된다.
- `[PUT] https://(도메인)/classes/1/students/6`
    - `BODY : {"idx":6, "name":"김이박", "grade":"B"},`
- 어느 학생의 정보들이 변경되었면, URI에 변경할 학생의 번호까지 담아서 PUT 또는 PATCH를 사용해서 변경, Update될 새 정보들을 body에 실어보낸다.
- PUT과 PATCH의 사용은 쓰는 곳마다 다르고 그냥 PUT으로 다 하는 곳들도 있다.
- 알려진 정석은
    - `PUT`은 정보를 통째로 갈아끼울 때,
    - `PATCH`는 정보 중 일부를 특정 방식으로 변경할때 사용한다.

---
## DELETE

- delete : 데이터를 delete, 정보를 삭제하는데 사용된다.
어떤 학생이 자퇴를 했다면,
`[DELETE] https://(도메인)/classes/1/students/6`

---

## RESTful하지 않은 API - 3

- 예를들어 POST로만 데이터를 처리한다면
    - `[POST] https://(도메인)/classes/1/students/create`
    - `[POST] https://(도메인)/classes/1/students/reade`
    - `[POST] https://(도메인)/classes/1/students/update`
    - `[POST] https://(도메인)/classes/1/students/delete`
    - 라고 따로 요청 내용을 표시해줘야 할 것이다.
- 학생들이 아니라 개별 학생을 다룬다면
    - `[POST] https://(도메인)/classes/1/students/create`
    - `[POST] https://(도메인)/classes/1/students/2/read`
    - `[POST] https://(도메인)/classes/1/students/3/update`
    - `[POST] https://(도메인)/classes/1/students/4/delete`
    - 이렇게 써야한다.
    - 뒤에 매번 덧붙이는 것이 깔끔하지가 않다.

---
## REST의 규칙 중 하나 - URI는 명사로

- 이런 귀찮은 일들을 지양하기 위한 REST의 규칙 중 하나로
URI는 동사가 아닌 명사들로 이뤄져야 한다는 것이 있다.
---

## 정리

- 결국 REST API란, HTTP 요청을 보낼 때, 어떤 URI에 어떤 메소드를 사용할 지(+기타) 개발자들 사이에 널리 지켜지는 약속.
- 형식이기 때문에 기술에 구애받지 않는다.
- 앱을 만들든, 웹사이트를 만들든, 어떤 언어로 뭘 써서 만들든 거기에 소프트웨어간에 HTTP로 정보를 주고받는 부분이 있다면 이 형식, 규칙들을 준수해서 RESTful한 서비스를 만들수 있다.
- 더 자세한 내용을 위한 검색 키워드 `restful api design guidelines` 

---

# 진짜 REST는 뭔데?

## REST의 시작 - WEB
Q: 어떻게 인터넷에서 정보를 공유할 것인가?
A: 정보들을 하이퍼텍스트로 연결한다.
    - 표현 방식 : HTML
    - 식별자 : URI
    - 전송방법 : HTTP

---
## HTTP/1.0(1994-1996)
>Roy T. Fiedling
>"How do I improve HTTP without breaking the Web?"

---

## REST

REpresentational 
State 
Transfer
- 분산 하이퍼미디어 시스템(예, WEB)을 위한 아키텍쳐 스타일

---

## REST API
- REST 아키텍쳐 스타일을 따르는 API

---

## REST를 구성하는 스타일
- client-server
- stateless
- cache
- uniform interface
- layered system
- code-on-demand(optional) : 자바스크립트

 HTTP만 잘 따라도 대체로 잘 지켜짐.
 -uniform interface만 제외하고

 ## Uniform Interface의 제약조건
 ---
 #### 잘 지켜지는 것
 - identification of resources : 리소스가 URI로 식별되면 된다.
 - manipulation of resources through representaions : representiation 전송을 통해서 리소스를 조작해야한다.

#### 잘 지켜지지 못하는 것
 - self-descriptive messages : 메시지는 스스로를 설명해야한다.
 - hypermedia as the engine of application state(HATEOAS) : 애플리케이션의 상태는 Hyperlink를 이용해 전이되어야한다. 
---

## 왜 Uniform Interface가 필요한가?
#### 독립적 진화
- 서버와 클라이언트가 각각 독립적으로 진화한다.
- 서버의 기능이 변경되어도 클라이언트를 업데이트할 필요가 없다.
- REST를 만들게 된 계기 : "HOW do I improve HTTP without breaking the Web."

---

## REST는 잘 지켜지고 있는가? 
#### WEB
- 웹 페이지를 변경했다고 웹 브라우저를 업데이트할 필요는 없다.
- 웹 브라우저를 업데이트했다고 웹 페이지를 변경할 필요도 없다.
- HTTP 명세가 변경되어도 웹은 잘 동작한다.
- HTML 명세가 변경되어도 웹은 잘 동작한다.
- 옛 브라우저에서 뷰가 깨져보일 수 있지만, 동작은 한다.

---
## REST가 잘 지켜지지 않고 있는 것은?
- 앱 업데이트
- Chrome 브라우저를 이용해주세요.
- IE를 이용해주세요.

---

## 그런데 REST API는?
- REST API는 REST 아키텍쳐 스타일을 따르는 API이다.
- 오늘날 스스로 REST API라고 하는 API들의 대부분이 REST 아키텍쳐 스타일을 따르지 않는다.


---

## REST API도 저 제약조건을 다 따라야하는가?
- Roy T. Feilding : 응
- 개발자 : 어렵네..
- Roy T. Feilding : 
    - __시스템 전체를 통제할 수 있다고 생각하거나,__
        = 당신이 서버 개발자고 클라이언트 개발자를 당신 마음대로 할 수 있다면
        = 당신이 클라이언트와 서버를 다 만든다면
    - __진화에 관심이 없다면,__
        = 뭐.. 욕먹어도 업데이트 시키면 된다는 마인드를 가지고 있다면
    - __REST에 대해 따지느라 시간을 낭비하지 마라__

- 즉, 오랜시간에 걸쳐 시스템을 설계하고 싶다면 REST에 관심을 가져라.

---

## 그럼 이제 어떻게 할까?
1. REST API를 구현하고 REST API라고 부른다.
2. REST API 구현을 포기하고 HTTP API라고 부른다.
3. __REST API가 아니지만 REST API라고 부른다. (현재 상태)__


Roy T. Feilding : 제발 제약조건을 따르던지 아니면 다른 단어를 써라.

---
## 왜 API는 REST가 잘 안되나
- API는 기계와 기계의 대화.
- Self-Descriptive Messages를 만족시키기 힘들다(귀찮다).