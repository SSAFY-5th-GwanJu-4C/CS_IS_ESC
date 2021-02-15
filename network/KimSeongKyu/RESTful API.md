# 1. REST란?

- **RE**presentational **S**tate **T**ransfer의 약자로 자원을 URI로 표현하여 해당 자원의 상태를 표현하는 방법론

---

## 1.1) REST의 구성요소

- 자원 - URI로 표현되며 Server에 저장되어 있음
- 행동 - HTTP Method로 사용되며 URI상에 표현되어서는 안됨
- 표현

---

## 1.2) REST의 특징

- Client-Server Architecture
- Stateless
- Cacheable
- Layered System - Client는 REST API Server만 호출하기 때문에 API Server는 Business Logic만을 수행하고 인증, 암호화, Load Balancing등 계층을 추가하여 구조상의 유연성을 둘 수 있음
- Uniform Interface - HTTP 표준만 따르면 언어 및 플랫폼에 종속되지 않고 사용할 수 있음
- Self-Descriptiveness - 자원(명사) + 행동(동사)로 표현되어 REST API 메세지만 보고도 쉽게 이해할 수 있음

---

# 2. REST API

## 2.1) REST API 설계 규칙

- URI는 정보의 자원을 표현하며 **동사가 아닌 명사**를 사용해야 함
- Collection는 복수로, Document는 단수로 표현
- 자원에 대한 행위는 HTTP METHOD로 표현
- '/'를 사용해 자원의 계층 관계를 표현
- URI의 마지막에는 '/'를 사용하면 안됨
- '-'를 사용해 긴 URI의 가독성을 높임
- '_'는 URI에 사용하지 않음
- URI문법 형식에 의해 대소문자가 구분되기 때문에 URI에는 소문자를 사용
- URI에 파일 확장자를 포함하지 않음

---

# 3. RESTful API

- REST를 REST답게 쓰자는 의미로 누군가 공식적으로 발표한 것이 아닌 "이해하기 쉽고 사용하기 쉬운 REST API를 만들자"는 목적의 개발자들 간의 용어

---

## 3.1) RESTful 하지 못한 경우

- CRUD기능을 전부 HTTP POST Method로 처리하는 경우
- URI에 자원과 id 이외의 정보가 들어가는 경우

---

References

[https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)<br>
[https://velog.io/@stampid/REST-API와-RESTful-API](https://velog.io/@stampid/REST-API%EC%99%80-RESTful-API)<br>
[https://brainbackdoor.tistory.com/53](https://brainbackdoor.tistory.com/53)
