# 1. OAuth란?

- 외부 서비스에서 제공하는 서비스를 이용하기 위한 인증 표준
- OAuth가 표준으로 자리잡기 전까지는 서비스마다 개별적인 인증 방식을 개발하여 사용했음
- ex) 네이버 로그인, 카카오 로그인, 구글 로그인 등

---

# 2. OpenID

- OpenID는 OAuth와 마찬가지로 인증을 위한 표준이지만 OAuth와의 목적이 다름
- OAuth는 `Authorization` (허가)를 목적으로 하기 때문에 서비스의 기능 사용을 목적으로 하는 인증 수단 → 인증을 해당 서비스의 Service Provider가 직접 처리
- OpenID는 `Authentication` (인증)을 목적으로 하기 때문에 서비스의 기능 사용 관련해서 신경쓰지 않음 → 인증을 서비스가 OpenID Service Provider에 위임하여 처리

---

# 3. OAuth1.0

## 3.1) 주요 용어

- Service Provider : OAuth를 사용해 Open API를 제공하는 서비스
- Consumer : OAuth 인증을 사용해 Service Provider의 기능을 사용하려는 웹 서비스
- User : Service Provider에 계정을 가지고 있으며 Consumer를 이용하려는 사용자
- Request Token : Consumer가 Service Provider에 접근 권한을 인증받기 위해 사용하는 값
- Access Token: 인증이 완료된 Request Token을 Access Token으로 교환하여 Consumer가 Service Provider에 접근하기 위한 키를 포함한 값

---

## 3.2) 인증 절차

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/network/KimSeongKyu/images/OAuth1.0%20%EC%9D%B8%EC%A6%9D%EC%A0%88%EC%B0%A8.png" width="400" height="600">

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/network/KimSeongKyu/images/OAuth1.0%20%EC%9D%B8%EC%A6%9D%EC%A0%88%EC%B0%A8%20%EA%B0%84%EB%9E%B5%ED%99%94.png" width="1000" height="300">

# 4. OAuth2.0

- OAuth1.0의 단점을 개선하여 만든 새로운 인증 표준

    🙋‍♂️ OAuth1.0의 단점이 무엇인가요?

    📢 웹이 아닌 다른 애플리케이션에서 사용하기 어려움 / 인증절차가 복잡하여 OAuth 구현 라이브러리 제작이 어려움 / 복잡한 철차로 인해 Service Provider에 부담이 됨

- 특이하게도 버전을 업그레이드하여 만들었지만 OAuth1.0과 호환이 되지 않음

---

## 4.1) 주요 용어

- Client : 사용자가 이용중인 서비스
- Resource Owner : 이용중인 서비스에서 외부 서비스에 접근하려는 서비스 사용자
- Authorization Server : 인증절차를 통해 Token을 발행해주는 서버
- Resource Server : 사용자가 이용하려는 외부 서비스

---

## 4.2) 인증 절차

<img src="https://github.com/KimSeongKyu/CS_IS_ESC/blob/KimSeongKyu/network/KimSeongKyu/images/OAuth2.0%20%EC%9D%B8%EC%A6%9D%EC%A0%88%EC%B0%A8.png" width="800" height="400">

---

[출처]

[https://api.slack.com/legacy/oauth](https://api.slack.com/legacy/oauth)<br>
[https://d2.naver.com/helloworld/24942](https://d2.naver.com/helloworld/24942)<br>
[https://baked-corn.tistory.com/29](https://baked-corn.tistory.com/29)<br>
