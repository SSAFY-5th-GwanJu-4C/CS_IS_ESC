# 1. HTTP / HTTPS 
### ▫ HTTP 
* Hyper Text Protocol의 약자로써 www(world wide web) 사에서 정보를 주고 받는 프로토콜 
* 텍스트 교환방식   
* 80 port 사용
* TCP와 UDP 사용

### ▫ HTTPS 
* HTTP의 텍스트 교환방식에서의 문제점인 네트워크 신호 노출 보안상 문제를 해결
* 인터넷 상에서 정보를 암호화하는 SSL(Secure Socket Layer) 프로토콜을 이용
* 클라이언트와 서버가 데이터 주고받는 통신 규약
* http보다 보안상 우위

    * **SSL /TLS**
    >  * 인터넷 상에서 데이터를 안전하게 전송하기 위한 인터넷 암호화 통신 프로토콜
    > * 데이터 송신할 때 http는 애플리케이션 계층에서 전송계층으로 보낸 후 송신
    > * **https** 는 애플리케이션 계층에서 SSL로 데이터를 전송하고 SSL은 받은 데이터를 암호화하여 전송계층으로 전달
    > * 송수신시에 **http** 는 TCP(전송계층)을 SSL로 인식하기 때문에 기존 방식 그대로 사용

<p align="center"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnsqZk%2FbtqCZ1vh5oi%2FI7dZCzIb7rI574X71WHdC1%2Fimg.png" width="50%" height="30%"></img></p>

### ▫ HTTPS 통신 방식
* 대칭키와 공개키 암호화 사용
* 대칭키 주고 받을 때 비대칭기 암호화를 통해 교환 
* 키를 교환한 후부터 서로 대칭키 이용해 암호화하여 데이터 주고 받는다.

    1. 클라이언트에서 랜덤한 데이터 만들고, 세션 ID, 클라이언트가 지원하는 암호화 방식들을 서버에 보냄
    
    2. 서버도 랜덤한 데이터를 만들고 서버가 사용할 암호화 방식과 인정서를 같이 클라이언트로 보냄
    
    3. 서버의 인증서가 CA에서 발금된 것인지 확인하기 위해 클라이언트에 내장된 CA리스트 확인.   
    CA리스트에 없다면 사용자에게 일단 경고 메시지 보여주고, 인증서가 CA에 의해서 발급된 것인지    
    확인하기 위해 클라이언트에 내장된 CA 공개키를 이용해서 인증서를 복호화   
    (복호화가 성공한다는 것은 CA의 개인키로 암호화된 문서임을 보증, 그렇기 때문에 인증서를 전송한 서버를 믿을 수 있음)
    
      ❔ 여기서 CA란?
      
      - Certificate Authority의 약자로 Root Ceritificate라고도 하며 인증 기관을 의미.
      - 클라이언트가 접속한 서버가 정상적인 서버가 맞는지 확인하여 맞다면 인증서를 제공   
     
      ❔ 복호화(decryption) ?
      
      - 암호화의 반대로 디코딩(decoding)이라고도 한다. 부호화(encoding)된 데이터를 부호(code)화 되기 전 형태로 바꾸어,   
      사람이 읽을 수 있는 형태로 되돌려 놓음
    
    4. 서로 교환한 랜덤 데이터를 조합해서 비밀키 생성. 비밀키를 서버에 전달할 때 공개키 방식응 이용해서 서버로 전송   
    공개키는 인증서 안에 있고, 공개키로 암호화된 데이터는 대칭이 되는 비공개키로만 복호화 가능   
    서버는 자신이 가진 비공개키고 복호화한다. (이렇게 되면 서로 비밀키가 공유된다.)
    
    5. 서로 비밀키를 교환했으므로 비밀키를 이용해서 암호화하여 실제 데이터 주고 받음
    
    6. 데이터 전송이 끝나면 통신에서 사용한 대칭키인 세션키 폐기후 연결 종료

### ▫ HTTP 통신 방식
* 요청(request) - 응답(response) 방식
    클라이언트(웹브라우저)가 요청을 서버에 보내면, 서버는 요청에 따른 처리 후 결과에 따른 HTTP 응답을 클라이언트에 보냄
* 비상태연결(Stateless, Connectless)
    HTTP통신은 state라는 개념이 없다. 요청하고 응답을 받으면 연결을 끊어버림. -> 서버와 클라이언트는 독립적   
    
    - 장점 : 접속 유지 최소화, 연결 상태 처리, 정보의 저장관리 불필요해 서버 디자인 간단   
    
    - 단점 : 접속이 유지되지 않기 때문에 이전 통신의 정보를 알 수 없음, 따라서 로그인 등과 같이 정보를 유지해야하는 경우에도 정보 유지 불가.
           (이를 해결하기 위해 쿠키, 세션등을 이용)
           
 ### ◾ HTTP 요청(request), 응답(response) 구조
 
 1) **요청 내용** : 요청 또는 요청 수행에 대한 성콩 또는 실패가 기록되며 이 줄은 항상 한줄로 끝남
 
 2) **헤더** : 옵션으로 헤더 세트가 들어간다. 여기에는 요청에 대한 설명, 혹은 메세지 본문에 대한 설명이 들어감
 
 3) **빈줄** : 요청에 대한 모든 메타 정보가 전송되었음을 알리는 빈줄
 
 4) **바디(body)** : 요청과 관련된 내용(HTML form 컨텐츠 등)이 옵션으로 들어가거나, 응답과 관련된 문서(document)가 들어간다.   
 본문의 존재 유무 및 크기는 요청 헤더에 명시
 
#### < 요청(request) 구조 >
<img src="https://user-images.githubusercontent.com/50662573/106912326-4355cc00-6746-11eb-9835-3bbcad60e1dd.png" width="100%" height="100%"></img>
 
 
#### < 응답(response) 구조 >
<img src="https://user-images.githubusercontent.com/50662573/106912673-93cd2980-6746-11eb-8012-99854fedb234.png" width="100%" height="100%"></img>


### ◾ HTTP 요청 메소드

: HTTP 메소드는 HTTP 통신 요청이 이루어질 때 어떠한 액션을 요청하는지 알려줌      


<img src="https://user-images.githubusercontent.com/50662573/106913064-f7efed80-6746-11eb-96ac-a14f18f45c8e.png" width="100%" height="100%"></img>
<참고>  
* POST / PUT : POST는 같은 작업을 반복해도 결과가 다르게 나오고, PUT은 같게 나온다.   
즉, 동일한 자원을 여러번 POST할 경우 서버 자원에 변화가 생기지만 PUT는 변화가 생기지 않는다.

* PUT / PATCH : PUT는 전체 자원을 수정할 때 동일한 수정일 경우 결과가 같고, PATCH의 경우 일부가 변경된다.   

### ◾ HTTP 응답 

: 클라이언트가 요청하면 서버는 결과를 응답 코드와 함께 응답

<img src="https://user-images.githubusercontent.com/50662573/106913875-cf1c2800-6747-11eb-9c71-e136ab7f60e4.png" width="100%" height="100%"></img>

### ◾ HTTP 주요 헤더 종류
* 공통 헤더 - 요청(request), 응답(response) 둘다 사용
  * Date : HTTP 메세지를 생성할 일시
  * Connection : 클라이언트와 서버 연결에 대한 설정
    * Connection: close : 통신 직후 TCP 접속 종료
    * Connection: Keep-Alice : 현재 TCP 커넥션 유지
    
  * Cache-Control
    * Cache-Control: no-store : 캐시 미사용
    * Cache-Control: no-cache : 서버에게 확인 후 사용
    * Cache-Control: must-revalidate : 만료된 캐시만 서버에서 확인
    * Cache-Control: public : 공유 캐시에 저장 가능
    * Cache-Control: private : '브라우저'같은 특정 사용자 환경에만 저장
    * Cache-Control: max-age : 캐시 유효시간을 명시
    
   * Pragma : HTTP/1.0에서 사용하던 캐시 제어
   * Content-Type, Content-Encoding, Content-Length, Content-Language : 본문 설정      
 
 
* 요청 헤더
  * Host : 요청하려는 url 정보
  * User-anget : 클라이언트 프로그램 정보 ex) 브라우저 정보
  * Referer : 직전에 머물러 있던 페이지 정보
  * Accept, Accept-charset, Accept-language, Accept-encoding : 클라이언트 정보
  * Authorization : 인증 토큰 정보
  * Origin : 맨 처음 시작 요청 주소값
  * Cookie : 쿠키 값     
  
  
  
* 응답 헤더
  * Location : 301,302에 사용되는 헤더로 변경된 주소를 지정
  * Server : 웹서버의 종류
  * Age : max-age 시간에서 얼마나 지났는지 (초단위)
