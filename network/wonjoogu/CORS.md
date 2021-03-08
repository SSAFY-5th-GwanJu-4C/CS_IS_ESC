# Cors란?

* Cross - origin resource sharing, CORS) **교차 출처 리소스(자원) 공유**
* **한 도메인에서 로드되어 다른 도메인에 있는 리소스와 상호 작용하는 클라이언트 웹 애플리케이션에 대한 방법**을 정의
* **웹 페이지 상의 제한된 리소스를 최초 자원이 서비스된 도메인 밖의 다른 도메인으로부터 요청할 수 있게 허용하는 구조** ***(cors가 구현된 웹브라우저에서는 다른 도메인 간 통신이 가능)***
* 특정한 도메인 간(cross - domain) 요청, 특히 Ajax 요청은 동일 - 출처 보안 정책에 의해 기본적으로 금지된다.
* 교차 출처 요청을 허용하는 것이 안전한지 아닌지를 판별하기 위해 브라우저와 서버가 상호 통신하는 하나의 방법을 정의

### Origin 이란?

오리진과 비슷한 개념으로는 도메인(domain)이 있다. 둘 사이의 구체적인 예로는 아래와 같다.
* 도메인(domain): naver.com
* 오리진(origin): https://www.naver.com/PORT
: 이와 같이 도메인과 오리진의 차이는 **프로토콜과 포트번호의 포함 여부**이다.

# Cors의 동작 원리
<p align="center"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcs4wto%2FbtqzZKoD3Mi%2FWcisEe9B1vsXyZCtzbO150%2Fimg.png" width="60%" height="60%"></img></p>   

1. 브라우저에서 다른 도메인의 서버로 options 메서드로 전송합니다.  

2. 서버의 설정이 access-control-allow-origin이 허용일 경우 OK를 리턴합니다.   

3. OK이며, 사용 가능한 http  메소드 라면, 요청하려는 API를 서버 측에 호출합니다. 

4. 서버에서 응답합니다.

> options으로 확인하는 것은 http  메소드 중 **멱등성이 없는 메소드**로 확인 및 리턴으로 해당 API로 사용할 수 있는 **메서드를 리턴하여 요청을 수행할 수 있는지를 확인**하기 위함입니다.  

# Cors - 왜 필요한가?
### Same-Origin Policy
* 웹 시큐리티의 중요한 정책 중 하나로 Same-Origin Policy가 있다.
이는 오리진 사이의 리소스 공유에 제한을 거는 것으로 다음과 같은 **위험을 막는 것을 목적**으로 하고 있다.

1. **XSS(Corss Site Scripting)**
유저가 웹 사이트에 접속하는 것으로 정상적이지 않은 요청이 클라이언트(웹 브라우저)에서 실행되는 것을 나타내며, 
Cookie 내에 Session정보를 탈취 당하는 등의 예시가 있다.

2. **CSRF(Cross-Site Request Forgeries)**
웹 어플리케이션의 유저가 의도하지 않은 처리를 웹 어플리케이션에서 실행되는 것을 나타내며, 
원래는 로그인한 유저 밖에 실행할 수 없는 처리가 멋대로 되는 등의 예시가 있다.

## CORS 구현하는 방법

> 먼저, 클라이언트 측에서는 특별히 할 게 없다. 별도의 오리진으로의 요청은 아래와 같이 'Origin' 이라는 필드를 추가하면 된다.

    Origin: https://xxx.co.kr

> Fetch API에서는 'cors mode'을 설정할 필요가 있다.

    fetch('https://xxx.co.kr', {
      mode: 'cors'
    });

> 다음, 서버 측에서는 응답 헤더를 아래와 같이 추가할 필요가 있다.

    Access-Control-Allow-Origin: https://xxx.co.kr  // 특정 사이트를 허가
    Access-Control-Allow-Origin: *   // 모든 사이트를 허가
    Access-Control-Allow-Headers "X-Requested-With, Origin, X-Csrftoken, Content-Type, Accept"  // 여기는 사용하는 프레임워크에 따라 달라진다.

### Cookie도 허가 하기

> 예를 들어 testA.com 이라는 페이지를 연 상태로 testB.com으로 XMLHttpRequest를 보낼 때,
testB.com의 Cookie도 포함해서 요청을 보내고 싶은 경우에는 기본적으로는 서로 다른 오리진에는 Cookie를 포함해서 보낼 수 없다.

이 경우에는 클라이언트 측과 서버 측 각각 이를 구현하기 위해 아래와 같은 설정이 필요하다.

#### 클라이언트 측

1. XHR을 사용하는 경우

        const xhr = new XMLHttpRequest();
        xhr.withCredentials = true; // 여기를 추가

2. Fetch API를 사용하는 경우

        fetch('https://trusted-api.co.jp', {
          mode: 'cors',
          credentials: 'include' // 여기를 추가
        });
    
3. axios를 사용하는 경우

        axios.get('https://trusted-api.co.jp', { 
          withCredentials: true
        });

        axios.defaults.withCredentials = true; // 글로벌에 설정한 경우
    
# REST API 리소스에 대한 CORS 활성화
CORS(Cross-origin 리소스 공유)는 브라우저에서 실행 중인 스크립트에서 시작되는 cross-origin HTTP 요청을 제한하는 브라우저 보안 기능입니다. 
**REST API의 리소스가 비 단순 cross-origin HTTP 요청을 받을 경우 CORS 지원을 활성화해야 합니다.**

## CORS 지원 활성화 여부 확인
: cross-origin HTTP 요청은 다음에 대해 이루어지는 요청입니다.
* 다른 도메인(예: example.com에서 amazondomains.com으로)
* 다른 하위 도메인(예: example.com에서 petstore.example.com으로)
* 다른 포트(예: example.com에서 example.com:10777으로)
* 다른 프로토콜(예: https://example.com에서 http://example.com으로)

>**Cross-origin HTTP 요청**은 **단순 요청**과 **비단순 요청**의 두 가지 유형으로 나눌 수 있습니다.

>다음과 같은 모든 조건을 충족하는 HTTP 요청은 **단순 요청**입니다.

* GET, HEAD, POST 요청만 허용하는 API 리소스에 대해 발행된 요청입니다.
* POST 메서드 요청인 경우 Origin 헤더를 포함해야 합니다.
* 요청 페이로드 콘텐츠 유형이 text/plain, multipart/form-data 또는 application/x-www-form-urlencoded입니다.
* 요청에 사용자 지정 헤더가 없습니다.
* 단순 요청에 대한 Mozilla CORS 문서에 나열된 추가 요청.

>단순 cross-origin POST 메서드 요청의 경우 해당 리소스로부터의 응답에 Access-Control-Allow-Origin 헤더가 포함되어야 합니다. 
헤더 키 값은 '*'(임의의 origin)으로 설정되거나, 해당 리소스에 대한 액세스가 허용되는 origin으로 설정됩니다.   

>다른 모든 cross-origin HTTP 요청은 비 단순 요청입니다. 
API의 리소스에서 비단순 요청을 수신하는 경우 CORS 지원을 활성화해야 합니다.

## CORS 지원 활성화의 의미

> 브라우저에서 비단순 HTTP 요청을 받을 경우 CORS 프로토콜은 브라우저에 preflight 요청을 서버로 보내고,
서버 승인(또는 자격 증명에 대한 요청)을 기다린 후 실제 요청을 보내도록 요구합니다.
preflight 요청은 API에 다음과 같은 HTTP 요청으로 나타납니다.

* Origin 헤더를 포함합니다.
* OPTIONS 메서드를 사용합니다.
* 다음 헤더를 포함합니다.
  * Access-Control-Request-Method
  * Access-Control-Request-Headers

> CORS를 지원하기 위해 REST API 리소스는 페치 표준에서 요구하는 다음과 같은 응답 헤더를 한 개 이상 포함하는
OPTIONS preflight 요청에 응답할 수 있는 OPTIONS 메서드를 구현해야 합니다.

* Access-Control-Allow-Methods
* Access-Control-Allow-Headers
* Access-Control-Allow-Origin
(CORS 지원을 활성화하는 방식은 API의 통합 유형에 따라 다릅니다.)





