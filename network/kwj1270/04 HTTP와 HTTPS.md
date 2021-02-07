# 📚 들어가면서      
                  
HTTPS의 등장이유에 대해서는 우리는 안다.                      
HTTP의 등장이유는?                        
그리고 다른 통신과의 차이점은?                        
                                  
요즘은 모든 것이 HTTP 기반 위에서 동작한다.                         
HTML, 이미지 , 영상, 파일뿐만 아니라 앱과 서버의 통신, 서버와 서버의 통신에서도 데이터를 주고 받고 있다.        
특히 백엔드 개발자는 웹 기술들과 웹프레임워크들을 사용할텐데 HTTP 기반으로 구현하고 있다.    
        
HTTP를 제대로 이해하지 못한 상태로 처음 웹 기술들을 공부하면 깊이있게 원리파악 힘듬    
해당 기술들은 이미 HTTP원리를 이미 잘 알고 있다고 가정하고 기능 사용법 위주로 설명함     
예를들어 : SpringMVC의 content negotiation     
이외에도 HTTP 에 관련된 용어들이 나올텐데 이를 잘 모르고 넘어갈 경우가 많음(가볍게 공부할 수 밖에없음)       
          
실무에서 웹 기술을 사용하는 개발자는 늘 고민한다.          
API URL는 어떻게 설계할까?, POST? PUT? HTTP 상태코드는 어떤걸 사용할까?          
그렇기 때문에 HTTP의 핵심 내용을 학습하고 자신만의 학습 기준을 세우고 싶은데       
인터넷 리서칭을 해보면, 자료들이 조각조각 나눠져있고, 잘못된 자료가 많음        
     
그렇다고해서 HTTP 스펙을 보니 모든 내용이 실무에 도움이될까? -> 아니다.      
      
개발자는 평생 HTTP 기반위에서 개발     
언젠가 한번은 HTTP 정리해야함     
  
# 📘 HTTP       
> Hypertext Transfer Protocol 의 약자로, 인터넷상에서 노드간에 데이터 통신을 위한 기초적인 프로토콜         
          
* Application 계층의 프로토콜     
* 인터넷상에서 `HyperText`를 교환하기 위해 사용되기 시작했다.    
* 기본적으로 80번 포트를 사용, WAS 같은 경우 8080을 사용한다.     
* **평문으로 데이터가 전송 된다. - 보안에 안 좋음**        
         
`HyperText`는 다른 파일에 대한 참조 링크인 **하이퍼링크**를 가지고 있으며,              
하이퍼링크를 통해 다른 문서로 이동하거나,        
그 페이지를 운영하는 서버에 데이터(Request)를 보낼 수 있다.           
    
대표적인 `HyperText`는 HTML이 존재하며,      
`JSON`, `XML`은 하이퍼링크와 비슷한 **하이퍼미디어**를 제공하기에 사용이 가능하다.          

**하이퍼 미디어와 하이퍼텍스트의 차이**   
```   
하이퍼미디어 정보는 이용자가 정보를 탐색할 때 어떤 제목에서 관련 제목으로 뛰어넘어 갈 수 있도록 연결되어 있다.      
연결된 정보가 주로 문자 정보로 되어 있으면 하이퍼텍스트이고,      
음악, 영상, 애니메이션 또는 다른 요소가 포함되어 있으면 하이퍼미디어가 된다.    
```
          
✔ 백엔드 개발자 같은 경우 HTML보다는 JSON과 XML을 통해 데이터를 통신하는데 주로 사용한다.                  
과거에는 TCP와 매우 밀접했지만, 현 트렌드가 비동기, 논블록킹으로 전환되면서 UDP와도 관계가 깊어졌다.                 
     
🤔 비동기와 UDP가 같은 의미인지 좀 헷갈리는데 이부분은 잡아주시면 감사하겠습니다.   
   
현재 대부분의 회사에서 모놀리식 아키텍처에서 MSA로 전환이 되고 있는데       
가장 대표적으로 사용하고 있는 기술이 바로, SpringCloud이다.       
SpringCloud는 Netflix OpenSource 를 기반으로 스프링에서 동작하게끔 만들어주고 있다.       
여기서 가장 중요한 것은 Spring API GateWay란 것이 SpringBoot 2.4버전에 등장했는데,       
기본적인 조건이 Java 비동기 통신인 Netty와 SpringWebFlux를 이용해야 한다는 것이다.           
     
SpringWebFlux는 Spring MVC 와 정반대 구조로 EventLoop로 인해 동작한다.   
       
## 📖 HTTP 통신 - Request & Response      

![RequestAndResponse.png](./images/RequestAndResponse.png)    
            
`Clinet & Server` 구조에서 이용되며 **클라이언트에서 요청(Request)를 보내고 서버는 응답(Response)로 응답해주는 방식이다.**                 
          
웹 서버는 모두 `HTTP Daemon`😈을 가지고 있는데,          
`HTTP Daemon`은 HTTP 요청을 기다리고 있다가 **요청이 들어오면 이를 처리하도록 설계되어 있다.**         

```
HTTP의 입장에서의 웹 브라우저는 서버에 요구를 전달하는 하나의 클라이언트이다.            
사용자가 URL을 입력하거나, 하이퍼텍스트 링크를 클릭 함으로써 데이터를 요구하면, 브라우저는 HTTP 요구를 URL에 적혀있는 IP 주소에 전달한다.            
지정된 서버상의 HTTP Daemon은 그 요구(Request)를 받아서, 필요한 작업이 있다면 처리를 한 뒤에 요구된 데이터를 찾아서 응답한다.             
   
주의점 : 파일이 아닙니다. 데이터를 요청 응답하는 것입니다.    
``` 
   
이전에 배웠던 관련 내용_TCP 를 이용한 HTTP 통신 : [TCP 3-way Handshake](https://github.com/SMART-EYEARS/network/blob/main/03%20TCP%EC%99%80%20UDP.md#-tcp-3-way-handshake), [TCP 4-way Handshake](https://github.com/SMART-EYEARS/network/blob/main/03%20TCP%EC%99%80%20UDP.md#-tcp-4-way-handshake)   
   
## 📖 HTTP Message(Request Message & Response Message)       
> 정말 잘 정리해주신 분이 계셔서 공유 : [ss-won](https://velog.io/@ss-won/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-HTTP-Message%EC%99%80-Status-Code)    
        
![RequestAndResponseMessage](./images/RequestAndResponseMessage.png)     

### 📄 Request Message
> 참고하기 좋은 동영상 : [생활코딩 RequestMessage](https://opentutorials.org/course/3385/21674)    
   
![RequestMessage.png](./images/RequestMessage.png)

<br>

* Host : 서버의 도메인 주소 (DNS)  
* Accpet : 브라우저가 처리할 수 있는 데이터의 형태   
* Accept-Language : 서버가 돌려주기로 예상된 언어
* Accept-Encoding : 브라우저가 처리할 수 있는 컨텐츠 인코딩 압축 방식
* Content-Length : 메세지의 본문 크기를 byte단위로 표시
* User-Agent : 사용자의 웹 브라우저 종류&버전 정보   
  
위 내용들은 아직, 외울 필요없다. 나중에 구글링을 통해 검색하면 된다.    
보다 자세한 내용은 [MDN Web Docs-HTTP 헤더](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers)

**🔖 RequestMessageHeader**  
`RequestMessageHeader == RequestLine + RequestHeader`  
HTTP Request Message는 Start Line, Headers, Message Body로 이루어져있다.      

**RequestLine**        
해당 요청 또는 응답에 대한 성공 또는 실패를 기록하며 항상 한줄로 끝난다.        
* **`[HTTP Method](#http-method)`+ `Request Target` + `Http protocol version`**   
    
**RequestHeader**   
다양한 요청 메타데이터 정보가 들어있으며, 크게 Request, General, Entity Header로 나눌 수 있다.     
      
![HTTP_Request_Headers.png](./images/HTTP_Request_Headers.png)          
       
* **General headers :**    
    요청 및 응답 메시지 모두에서 사용 가능한 일반 목적의 헤더 항목          
* **Request headers :**      
    Request Message에서만 나타난다.            
    요청을 구체화 시키거나, context 제공, 또는 제약사항 등이 기재된다.        
* **Entity headers :**       
    Reqest, Response에서 모두 사용 가능한 Entity(콘텐츠, 본문, 리소스 등)에 대한 설명 부분        
    만약 본문내용이 없는 요청이라면 Entity 헤더는 전송되지 않는다.             
   
**🔖 Request Message Body**              
요청과 관련된 내용(HTML 폼 콘텐츠 등)이 옵션으로 들어가거나, 응답과 관련된 문서(document)가 들어간다.     
본문의 존재 유무 및 크기는 첫 줄과 Requestheader에 명시된다.         
`POST` 요청의 경우 **업데이트를 하기 위해 서버에 데이터를 전송한다.**                   
`GET`, `HEAD`, `DELETE` , `OPTIONS`처럼 **리소스를 가져오는 요청은 보통 본문이 필요가 없다.**       
쉽게 설명하면, 데이터를 전송하려면 body 이용, 가져오려면 header 에 존재하는 url에 쿼리 이용           
`RequestMessageHeader`와 `RequesetMeessageBody` 사이에는 한 줄의 공백이 있다.                     
  
**📌 정리**      
* **`HTTP Head` = `start-line` + `HTTP Headers`**
* **`HTTP Body` = `payload (실질적으로 전송의 목적이 되는 데이터 부분)`**

### 📄 Response Message   
> 참고하기 좋은 동영상 : [생활코딩 ResponseMessage](https://opentutorials.org/course/3385/21675)    
   
![ResponseMessage.png](./images/ResponseMessage.png)    
  
**🔖 ResponseMessageHeader**       
**ResponseLine**          
`HTTP Version + Status Code + Status Text`로 구성된다.     
상태 코드는 성공 및 실패의 여부를 나타내며, 상태 텍스트는 상태 코드에 대한 간결한 설명을 나타낸다.    
   
**ResponseHeader**   
다양한 응답 메타데이터 정보가 들어있으며, 크게 `Response`, `General`, `Entity Header`로 나눌 수 있다.     
     
![HTTP_Response_Headers.png](./images/HTTP_Response_Headers.png)        
          
General, Entity Header는 요청 메세지와 동일하며      
Response Header에는 **상태 텍스트와 코드에서 미처 나타내지 못한 서버의 메타데이타 정보를 담고 있다.**    
      
**🔖 Request Message Body**                     
모든 응답에 본문이 들어가지는 않는다.             
길이를 아는 단일-리소스 본문, 길이를 모르는 단일-리소스 본문, 그리고 다중 리소스 본문으로 나눌 수 있다.          
길이를 모르는 단일-리소스 본문에는 `Transfer-Encoding`가 `chunked`로 설정되어 있으며, 파일은 청크로 나뉘어 인코딩 되어 있다.       
   
## 📖 HTTP 1.0 과 HTTP 1.1 그리고 HTTP 2.0의 차이        
HTTP는 기본적으로 MIME 형태로 이루어지며 request/response의 방식에 기반을 두고 있다.            
HTTP는 원래 0.9v 부터 시작되었다고 하지만, 사실상 1.0버전이 상용화 되어 1996년부터 사용되기 시작했다.    
     
### 📄 HTTP 1.0 과 문제점
* HTTP 1.0은 단순하게 `open/operation/close`의 방식을 취하고 있어서 **단순하다.**      
* `TCP connection`당 하나의 URL만 fetch하며 **매번의 `request/response`가 끝나면 연결이 끊긴다.**          
* 그러므로 **매 번 필요할 때 마다 다시 연결을 해야 하므로 속도가 떨어진다.**             
* 또한, 한번에 얻어서 가져올 수 있는 **데이터의 양이 제한**되어 있다. 나아가 **URL의 크기도 작다.**       
             
즉, **매번 Conncetion/Close 작업을 해야하며 전송할 데이터량이 작다, URL 크기도 작다.**     

HTTP 1.0에서는 `open/close`를 위한 `flow`의 제한으로 대역폭이 적게 할당되어 연결되는데,    
이로 인해 [congestion information](https://ko.wikipedia.org/wiki/%ED%98%BC%EC%9E%A1_%EC%A0%9C%EC%96%B4)이 자주 발생하고 `disconnect`가 반복적으로 나타나게 된다.   
반복되는 `disconnect`현상으로 인해 **한 서버에 계속해서 접속을 시도하게 되면 과부하가 걸리고 성능이 떨어지게 되는 문제가 발생한다.**    
이런 문제를 해결하기 위해 `HTTP 1.1`이 등장한 것이다.   

### 📄 HTTP 1.1 과 문제점
HTTP의 인터넷에서 impact를 줄이고 **cache**를 두어 인터넷 프로토콜 수행이 빠르게 될 수 있도록 성능을 향상하고 있다.     
`multiple request`에 대한 처리가 가능하고 `request/response`가 파이프라인 방식으로 진행이 가능하다.        
   
![HttpVerOnedotOne.png](./images/HttpVerOnedotOne.png)    
      
맨왼쪽이 **HTTP_1.0**, **오른쪽 2개가 HTTP_1.1 이다.**                  
`multiple request`란 한번의 연결에 여러 Request를 보낼 수 있다는 뜻이고 (2번)          
`파이프라인 방식`이란, 여러 Reqeust를 한번에 보내고 한번에 응답받는 것을 말한다. (3번)           
         
이러한 형태를 `Keep-alive`라 부르며, 한 번 맺어졌던 연결을 끊지 않고 지속적으로 유지하여         
**불필요한 `hand-shake`를 줄여 성능을 개선할 수 있다.**               
            
하지만, 이런 `HTTP_1.1`에도 문제가 존재한다.       
만약, `파이프라인 방식`에서 처음에 요청한 `Request`에 문제가 있어서, 응답이 늦어지면     
2번째, 3번째에 요청한 `Request`의 응답도 같이 늦어진다는 문제점도 발생한다.      
이를 **`Head Of Line Blocking`이라고 부른다.**         
   
### 📄 HTTP 2의 등장    
 
![HTTPMultiPlexing.png](./images/HTTPMultiPlexing.png)
  
앞서 말했듯이, `HTTP_1.1`에는 `Request`에 문제가 있으면 응답이 늦어지는 **`Head Of Line Blocking`이 발생한다.**    
HTTP 2에서는 이를 해결한 `MultiPlexing` 이라는 개념이 도입되었다.       
   
위 그림 맨 오른쪽의 형태처럼    
요청과 응답의 순서와 상관없이 먼저 끝나는순으로 Client에서는 응답을 받는 구조이다.      

이 외에도 여러 기능이 추가되었지만, 잘 이해가 가지않고 너무 deep dive하는 것 같아서 여기까지만 정리하겠다.    
     
# 📗 HTTPS      
`HTTPS`는 `HTTP 통신`에 인터넷 상에서 정보를 암호화하는 `SSL(Secure Socket Layer)`프로토콜을 이용하여       
클라이언트와 서버가 데이터를 주고 받는 통신 규약이다. (서버와 서버도 포함된다.)               
HTTPS는 HTTP와 다르게 433번 포트를 사용하며,        
네트워크 상에서 중간에 제3자가 정보를 볼 수 없도록 **공개키 암호화를 지원하고 있다.**       
 
![RequestMessage.png](./images/RequestMessage.png)    
 
앞서 보았던 RequestMessage 를 보면 평문으로 된 데이터를 그대로 보내는 것을 알 수 있다.     
   
## 📖 공개키 방식   
공개키 암호화 방식이란 쉽게 설명하면,                 
공개키는 누구나 알 수 있도록 공개하지만, 복호화할 때의 키는 나만 가지고 있는 비밀키로 유지한다.     
즉, 공개키에 대응하는 비밀키는 키의 소유자만이 알 수 있어서 특정한 비밀키를 가지는 사용자만이 내용을 열어볼 수 있도록 하는 방식이다.    
(동일한 공개키로, 동일한 데이터를 넣는다 하더라도 개인키가 서로 다르기 때문에 다른 해석 결과가 나온다.)           
이러한, 암호화 키와 복호화 키가 다른 모습에 `비대칭 키`라고도 부른다.      
      
송신자는 수신자의 공개키를 받아 데이터를 암호화하여 네트워크를 통해 원격지에 전달.    
수신자는 공개키로 암호화된 데이터를 자신의 개인키로 데이터를 복호화하여 평문을 복원.   

# 📕 REST API    
REST 는 웹과 같은 분산 하이퍼미디어 시스템에서 사용하는 통신 네트워크 아키텍처로, 네트워크 아키텍처의 원리 모음이다.       
             
웹은 전송 방식으로 HTTP, **식별 방법으로 URI를 사용한다.**            
* HTTP : 웹에서 `GET, POST, PUT, DELETE`등의 메소드를 사용하여 정보를 주고받는 프로토콜이다.             
* REST : HTTP 와 URI의 단순하고 간결한 장점을 계승화한 **네트워크 아키텍처**입니다.              
       
따라서 다양한 요구사항에 대응하여 때로는 단순하게,       
때로는 서버와 클라이언트가 서로 통신하는 리소스에 대해 복잡한 방식으로 상호작용할 수 있다.                   
           
REST는 다음과 같은 목적으로 만들어졌다.      
        
* 구성요소 상호작용의 규모 확장성        
* 인터페이스 범용성            
* 구성요소의 독립적인 배포          
* 중간적 구성요소를 이용한 응답 지연 감소, 보안 강화, 레거시 시스템 캡슐화          
      
**URI(Uniform Resouce Identifier, 통합자원식별자)**    
```
URI(Uniform Resouce Identifier, 통합자원식별자)는 인터넷에서 특정 자원을 나타내는 유일한 주소 값.   
REST API 는 URI를 사용하여 자원을 처리한다.     
```
   
## 📖 RESTful 제약조건       
REST의 구현 원칙을 제대로 지키면서 REST 아키텍처를 만드는 것을 RESTful 이라 말한다.             
   
다음은 RESTful 제약조건이다.         
          
* **클라이언트-서버** :             
이 제약 조건의 기본 원칙은 관심사의 명확한 분리를 뜻한다.             
관심사의 명확한 분리가 선행되면 서버의 구성요소가 단순화되고 확장성이 향상되어 여러 플랫폼을 지원할 수 있다.          
           
* **무상태성** :        
서버에 **클라이언트의 상태 정보를 저장하지 않는 것**을 의미한다.                  
**단순히 들어오는 요청만 처리**하여 구현을 더 단순화 합니다.                 
단, 클라이언트의 모든 요청은 **서버가 요청을 알아듣는데 필요한 모든 정보를 담고 있어야 한다.**             
           
* **캐시 기능** :     
**클라이언트의 응답을 캐시할 수 있어야 한다.**           
앞에서 HTTP의 장점을 그대로 계승한 아키택처가 REST라고 했습니다.            
따라서 HTTP의 캐시 기능도 적용할 수 있어야 한다.        
         
* **계층화 시스템** :       
서버는 중계 서버(게이트웨이, 프록시 서버)나 로드 밸런싱,      
공유 캐시등의 기능을 사용하여 확장성 있는 시스템을 구성할 수 있습니다.        
이 제약조건은 선택적 제약 조건이다.            
         
* **코드 온 디맨드** :      
클라이언트는 서버에서 자바 애플릿, 자바스크립트 같은 실행 코드를 전송받아 기능을 일시적으로 확장할 수 있다.   
이 제약조건은 선택적 제약 조건이다.            
         
* **인터페이스 일관성** :      
URI로 지정된 리소스에 균일하고 통일된 인터페이스를 제공한다.           
**아키텍처를 단순하게 분리하여 단순하게 만들 수 있습니다.**         

### 📄 인터페이스 일관성          
인터페이스 일관성은 세부 원칙을 가지고 있다.         
인터페이스 일관성이 잘 지켜졌는지에 따라 REST를 제대로 사용했는지 판단할 수 있다.      
인터페이스 일관성에는 다음 4가지가 존재한다.        
         
1. 자원 식별                  
2. 메시지를 통한 리소스 조작             
3. 자기 서술적 메시지           
4. 애플리케이션 상태에 대한 엔진으로서의 하이퍼미디어               
    
___ 
    
**자원 식별** :          
웹 기반의 REST에서 리소스 접근을 주로 **URI**을 사용한다는 것을 나타냅니다.         
즉, 각각의 리소스는 요청에서 식별가능합니다.        
   
```
http://localhost:8080/resource/1
```
예를 들어 URI에서 정보는 ```resouce```, 유일한 구분자는 ```1```로 식별합니다.         
   
___
   
**메시지를 통한 리소스 조작** :       
클라이언트가 특정 메시지나 메타데이터를 가지고 있으면       
자원을 수정, 삭제하는 충분한 정보를 갖고 있는 것으로 볼 수 있다.           
   
```
http://localhost:8080/resource/1
content-type: application/json    
```   
예를 들어 코드에서 ```content-type``` 은 리소스가 어떤 형식인지 지정한다.    
리소스는 HTML, XML, JSON 등 다양한 형식으로 전송된다.     
    
___    
        
**자기 서술적 메시지** :      
각 메시지는 자신을 어떻게 처리해야 하는지 충분한 정보를 포함해야 한다.       
웹 기반의 REST에서는 `HTTP Method`와 `Header`를 활용한다.          
   
```
GET http://localhost:8080/resource/1     // RequestMessageHeader -> requestLine의 HTTP Method  
content-type: application/json      
```   
예를 들어 GET 메서드를 활용하여 `/resource/1`의 정보를 받아온다는 것을 표현했다.         
    
___    
    
**애플리케이션 상태에 대한 엔진으로서의 하이퍼미디어** :        
> HATEOAS   
   
애플리케이션 상태에 대한 엔진으로서의 **하이퍼미디어**         
이는 클라이언트에 응답할 때 단순히 **결과 데이터만 제공해주기보다는 URI를 함께 제공해야한다는 원칙**이다.        
하이퍼텍스트 링크처럼 **관련된 리소스 정보를 포함한다.**      
          
REST의 제약조건들을 제대로 지키면서 REST 아키텍처를 만드는 것을 RESTful 이라고 한다.    
특히 제약 조건 중 **하이퍼텍스트를 포함**하고 **자기 서술적**이며 **인터페이스 일관성을 통해 리소스에 접근하는 API**여야 RESTFful 하다 할 수 있다.  
즉, 자기 자신이 어떻게 처리되는지에 대한 충분한 정보, 관련된 리소스를 하이퍼텍스트 링크로 포함시켜야 'RESTful 하구나' 생각할 수 있다.         
              
여기서 HATEOAS는 **응답 데이터와 관련된 하이퍼미디어 링크를 추가**하여 사이트의 REST 인터페이스를 **동적으로 탐색**하도록 도와준다.                 
               
* 클라이언트는 관련된 특정 동작에 따라 탐색할 만한 URI 값을 알 수 있다.                 
URI는 resource 까지 포함하므로 더 명확하게 예측이 가능하다.                
     
* 키 값이 변하지 않는 한 URI가 변경되더라도 동적으로 사용할 수 있다.         
따라서 서버쪽 코드가 변하더라도 클라이언트 코드를 따로 수정할 필요가 없다.        

[예시](https://velog.io/@jayjay28/HATEOAS)    
  
# 참고 
[sdc337dc님의 블로그](https://velog.io/@sdc337dc/%EC%9B%B9-%EA%B0%9C%EB%85%90-Http-%ED%86%B5%EC%8B%A0)       
[ss-won님의 블로그](https://velog.io/@ss-won/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-HTTP-Message%EC%99%80-Status-Code)     







