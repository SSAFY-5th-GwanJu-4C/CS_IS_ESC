# REST API란 ?

* REST API는 REpresentational State Transfer의 약자로 REST 기반으로 서비스 API를 구현한 것이다.
  
  * REST는 소프트웨어 프로그램 개발의 아키텍처의 한 형식
  * HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고, HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것을 의미   
  
-> CRUD Operation
* Create : 생성 (POST)
* Read : 조회 (GET)
* Update : 수정 (PUT)
* Delete : 삭제 (DELETE)
* Head : header 정보 조회 (HEAD)
  
### REST 의 장단점

* 장점
1. HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구출할 필요가 없다.
2. HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해준다.
3. HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
4. Hypermedia API의 기본을 충실히 지키면서 범용성을 보장
5. REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.
6. 여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화한다.
7. 서버와 클라이언트의 역할을 명확하게 분리한다.

* 단점
1. 표준이 존재하지 않는다.
2. 사용할 수 있는 메서드가 4가지 밖에 없다.
2-1. HTTP Method 형태가 제한적이다.
3. 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 값이 왠지 더 어렵게 느껴진다.
4. 구형 브라우저가 아직 제대로 지원해주지 못하는 부분이 존재한다.
4-1. PUT, DELETE를 사용하지 못하는 점
4-2. pushState를 지원하지 않는 점

#### 구성요소

* 자원(Resource) : URI
  * 모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재한다.
  * 자원을 구별하는 ID는 '/groups/:group_id'와 같은 HTTP URI다.
  * Client는 URI 를 이용해서 자원을 지정하고 해당 자원의 상태(정보)에 대한 조작을 Server에 요청한다.
  
* 행위(Verb) : HTTP Method
  * HTTP 프로토콜의 Method를 사용한다.
  * HTTP 프로토콜의 GET, POST, PUT, DELETE와 같은 메서드를 제공한다.
  
* 표현(Representation of Resource)
  * Client가 자원의 상태(정보)에 대한 조작을 요청하면 Server는 이에 적절한 응답(Representation)을 보낸다.
  * REST에서 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 Representation으로 나타내어질 수 있다.
  * JSON 혹은 XML를 통해 데이터를 주고 받는 것이 일반적이다.
  
## REST API 특징

>REST 기반으로 시스템을 분산해 확장성과 재사용성을 높여 유지보수 및 운용을 편리하게 할 수 있다.
REST는 HTTP 표준을 기반으로 구현하므로, HTTP를 지원하는 프로그램 언어로 클라이언트, 서버를 구현할 수 있다.
즉, REST API를 제작하면 델파이 클라이언트 뿐만 아니라 자바, C#, 웹 등을 이용해 클라이언트를 제작할 수 있다.   

1. Uniform Interface(일관된 인터페이스)

* Resource(URI)에 대한 요청을 통일되고, 한정적으로 수행하는 아키텍처 스타일을 의미
* 요청을 하는 Client가 플랫폼(Android, Ios, Jsp 등) 에 무관하며, 특정 언어나 기술에 종속받지 않는 특징
* 이러한 특징 덕분에 Rest API는 HTTP를 사용하는 모든 플랫폼에서 요청가능하며, Loosely Coupling(느슨한 결함) 형태를 갖는다. 

2. Stateless(무상태성)

* 서버는 각각의 요청을 별개의 것으로 인식하고 처리해야하며, 이전 요청이 다음 요청에 연관되어서는 안된다. 
* Rest API는 세션정보나 쿠키정보를 활용하여 작업을 위한 상태정보를 저장 및 관리하지 않는다. 
* 무상태성때문에 Rest API는 서비스의 자유도가 높으며, 서버에서 불필요한 정보를 관리하지 않으므로 구현이 단순하다. 
* 무상태성은 서버의 처리방식에 일관성을 부여하고, 서버의 부담을 줄여준다.

3. Cacheable(캐시 가능)

* Rest API는 결국 HTTP라는 기존의 웹표준을 그대로 사용하기 때문에, 웹의 기존 인프라를 그대로 활용할 수 있다. 
* 그러므로 Rest API에서도 캐싱 기능을 적용할 수 있는데, HTTP 프로토콜 표준에서 사용하는 Last-Modified Tag 또는 E-Tag를 이용하여 캐싱을 구현할 수 있고,
이것은 대량의 요청을 효울척으로 처리할 수 있도록 한다. 

4. Client-Server Architecture (서버-클라이언트 구조)

* Rest API에서 자원을 가지고 있는 쪽이 서버, 자원을 요청하는 쪽이 클라이언트에 해당한다. 
서버는 API를 제공하며, 클라이언트는 사용자 인증, Context(세션, 로그인 정보) 등을 직접 관리하는 등 역할을 확실히 구분시킴으로써 서로 간의 의존성을 줄인다. 

5. Self-Descriptiveness(자체 표현)

* Rest API는 요청 메세지만 보고도 이를 쉽게 이해할 수 있는 자체 표현 구조로 되어있다. 
* 아래와 같은 JSON 형태의 Rest 메세지는 http://localhost:8080/insertBoardInfo 로 게시글의 제목, 내용을 전달하고 있음을 쉽게 알수 있다. 


        HTTP POST , http://localhost:8080/insertBoardInfo

        { 

        "boardVO":{

                "title":"제목",

                "content":"내용"

                }

        }

6. Layered System(계층 구조)

* Rest API의 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 등을 위한 계층을 추가하여 구조를 변경할 수 있다. 
* 또한 Proxy, Gateway와 같은 네트워크 기반의 중간매체를 사용할 수 있게 해준다. 
* 하지만 클라이언트는 서버와 직접 통신하는지, 중간 서버와 통신하는지 알 수 없다.


## REST API 설계 기본 규칙

* URI는 저보의 자원을 표현
  * resource는 동사보다는 명사를, 대문자보다는 소문자 사용
  * resource의 도큐먼트 이름으로는 단수 명사 사용
  * resource의 컬렉션 이름으로는 복수 명사 사용
  * resource의 스토어 이름으로는 복수 명사 사용
  
* 자원에 대한 행위는 HTTP Method(GET, PUT, POST, DELETE 등)로 표현

  * URI에 HTTP Method가 들어가면 안된다.
  * URI에 행위에 대한 동사 표현이 들어가면 안된다. (즉, CRUD 기능을 나타내는 것은 URI에 사용하지 않는다.)
  * 경로 부분 중 변하는 부분은 유일한 값을 대체한다.

<p align="center"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdyBifh%2FbtqDIRTrDwD%2FCdClNv3DucPFCBnBmFe6ok%2Fimg.png" width="70%" height="70%"></img></p>

### RESTful 이란?

* RESTful은 일반적으로 REST라는 아키텍처를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어이다.
* RESTful은 REST를 REST답게 쓰기 위한 방법으로, REST원리를 따르는 시스템을 RESTful이란 용어로 지칭된다.

### RESTful 목적

* 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것
* RESTful한 API를 구현하는 근본적인 목적이 성능 향상에 있는 것이 아니라 일관적인 컨벤션을 통한 API의 이해도 호환성을 높이는 것이 주 동기이니,
성능이 중요한 상황에서는 굳이 RESTful한 API를 구현할 필요는 없다.

#### RESTful API 설계 가이드 잘 설명되어 있는 블로그 : https://library.gabia.com/contents/8339

